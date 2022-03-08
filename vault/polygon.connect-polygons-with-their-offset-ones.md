---
id: 6xcpu4fh3at4bbmptj9c8nr
title: Connect Polygons with Their Offset Ones
desc: ''
updated: 1646727539146
created: 1646726929486
---

## Steps:
1. Get list of edges for exterior and interior polygons
2. Force exterior to be CCW, interior to be CW
3. Apply delaunay triangulation
4. For each delaunay triangle, find the ones that do not intersect with exterior/interior polygons, append its non-polygon edges
5. Find all chordless cycles in undirected graph using wall-walking algorithm (ignore CW traversal of boundary edges)
6. Treat all chordless cycles as faces. For non-triangle faces, tessellated them if necessary

## Implementation
```
import numpy as np
from scipy.spatial import Delaunay
from rtree.index import Index
from pyclipper import PointsInPolygon, scale_to_clipper


def gen_loop_edges(polygon, offset):
    vertex_indices = np.arange(len(polygon))
    edges = np.vstack([vertex_indices, np.roll(vertex_indices, -1, axis=0)]).T
    return edges + offset


def gen_poly_edge_indices(polygons):
    offsets = np.insert(np.cumsum([len(poly) for poly in polygons])[:-1],
                        obj=0,
                        values=0)

    return [
        gen_loop_edges(poly, offset)
        for (poly, offset) in zip(polygons, offsets)
    ]


def gen_poly_edges(polygons):
    poly_edge_indices = gen_poly_edge_indices(polygons)
    return np.vstack(polygons)[np.vstack(poly_edge_indices)]


def gen_poly_rtree(polygons):
    poly_edges = gen_poly_edges(polygons)
    edge_bboxes = np.sort(poly_edges, axis=1).reshape(-1, 4)
    ids = np.arange(len(edge_bboxes))

    poly_rtree = Index()
    for (id, bbox) in zip(ids, edge_bboxes):
        poly_rtree.insert(id, tuple(bbox))

    return poly_edges, poly_rtree


# Reference: https://stackoverflow.com/questions/3838329/how-can-i-check-if-two-segments-intersect
def ccw(A, B, C):
    return (C[:, 1] - A[:, 1]) * (B[:, 0] - A[:, 0]) > (B[:, 1] - A[:, 1]) * (
        C[:, 0] - A[:, 0])


def intersect(A, B, C, D):
    return np.all([ccw(A, C, D) != ccw(B, C, D),
                   ccw(A, B, C) != ccw(A, B, D)],
                  axis=0)


def edge_intersect(key_edge, query_edges):

    key_edge_tiled = np.repeat(key_edge.reshape(1, 2, 2),
                               len(query_edges),
                               axis=0)

    disconnected = np.sum(np.isin(query_edges, key_edge_tiled).reshape(-1, 4),
                          axis=1) == 0

    if np.sum(disconnected) == 0:
        return False

    intersect_check = intersect(key_edge_tiled[disconnected][:, 0, :],
                                key_edge_tiled[disconnected][:, 1, :],
                                query_edges[disconnected][:, 0, :],
                                query_edges[disconnected][:, 1, :])

    if np.sum(intersect_check) > 0:
        return True
    else:
        return False


def adjaceny_list(edges, vertex):
    idx, _ = np.where(edges == vertex)
    query = edges[idx]
    return query[query != vertex].tolist()


# Reference: https://stackoverflow.com/questions/838076/small-cycle-finding-in-a-planar-graph
def find_all_chordless_cycles(all_verts, adjaceny_lists, excluded_edges):

    all_cycles = []
    edge_occurrence_set = set()

    # inner edges shall be traverse twice, while edges belong to the boundary of polygons shall only be travered once
    # if boundary edges were traversed twice, we end up getting boundary of polygons
    for edge in excluded_edges:
        edge_occurrence_set.add((edge[0], edge[1]))

    def find_chordless_cycles(vertex, cycle):
        adj_verts = adjaceny_lists[vertex]

        # filter out edges that already traversed
        adj_verts = list(
            filter(
                lambda adj_vertex:
                (vertex, adj_vertex) not in edge_occurrence_set, adj_verts))

        if len(cycle) == 0 and len(adj_verts) > 0:
            find_chordless_cycles(adj_verts[0], [vertex])
        elif len(cycle) > 0:
            edge_occurrence_set.add((cycle[-1], vertex))
            # don't want to link back
            adj_verts = list(
                filter(lambda adj_vertex: adj_vertex != cycle[-1], adj_verts))

            cycle_found = False
            if vertex in cycle:
                cycle_found = True
                all_cycles.append(cycle[cycle.index(vertex):])

            if len(adj_verts) > 0:
                dir_forward = all_verts[adj_verts] - all_verts[vertex]
                dir_back = np.tile(all_verts[cycle[-1]] - all_verts[vertex],
                                   [len(dir_forward), 1])
                # angle between, clip for robustness
                angle = np.arccos(
                    np.clip(
                        np.sum(dir_forward * dir_back, axis=1) /
                        (np.linalg.norm(dir_forward, axis=1) *
                         np.linalg.norm(dir_back, axis=1) + 1e-13),
                        -1.0 + 1e-13, 1.0 - 1e-13))

                # sign, positive if at left (CCW), negative if at right (CW)
                opposite = np.sum(
                    np.cross(np.tile([0, 0, 1], [len(dir_forward), 1]),
                             np.insert(dir_back, 2, 0, axis=1))[:, :2] *
                    dir_forward,
                    axis=1) < 0
                angle[opposite] = 2 * np.pi - angle[opposite]

                find_chordless_cycles(adj_verts[np.argmin(angle)],
                                      [vertex] if cycle_found else cycle +
                                      [vertex])

    for i in range(len(all_verts)):
        find_chordless_cycles(i, [])

    return all_cycles


def filter_intersecting_triangles(pt, tri, polygons):
    verts = pt[tri]
    verts_next = np.roll(verts, -1, axis=1)
    edges = np.concatenate([verts, verts_next], axis=-1).reshape(-1, 3, 2, 2)
    # we are only interested in edges that does not belong to a polygon
    non_edge_indices = np.abs(np.diff(tri, append=tri[:, 0].reshape(-1,
                                                                    1))) != 1

    query_edges = edges[non_edge_indices]
    query_bboxes = np.sort(query_edges, axis=1).reshape(-1, 4)
    unique_query_bboxes, unique_index, unique_inverse_index = np.unique(
        query_bboxes, return_index=True, return_inverse=True, axis=0)
    unique_query_edges = query_edges[unique_index]

    # not thread safe, cannot be reused
    poly_edges, poly_rtree = gen_poly_rtree(polygons)

    query_results = np.array([
        edge_intersect(key_edge, poly_edges[query_indices])
        for (key_edge, query_indices) in zip(unique_query_edges, [
            list(poly_rtree.intersection(bbox)) for bbox in unique_query_bboxes
        ])
    ])

    edge_intersection_results = np.full_like(tri, False, dtype=bool)
    edge_intersection_results[non_edge_indices] = query_results[
        unique_inverse_index]

    return np.sum(edge_intersection_results, axis=1) > 0


def filter_auxiliary_edges(pt, tri, polygons):
    poly_edges, poly_rtree = gen_poly_rtree(polygons)

    verts = pt[tri]
    verts_next = np.roll(verts, -1, axis=1)
    # filltering needs to be performed at triangle level
    edges = np.concatenate([verts, verts_next], axis=-1).reshape(-1, 3, 2, 2)

    edge_indices = np.concatenate([tri, np.roll(tri, -1, axis=1)],
                                  axis=-1).reshape(-1, 3, 2)

    # we are only interested in edges that does not belong to a polygon
    # first filter out consecutive edges
    non_edge_indices = np.abs(np.diff(tri, append=tri[:, 0].reshape(-1,
                                                                    1))) != 1

    # then filter out polygon last edges
    poly_edge_indices = gen_poly_edge_indices(polygons)
    polygon_last_edges = [
        edge_index[-1].tolist() for edge_index in poly_edge_indices
    ]
    edge_indices_list = edge_indices[non_edge_indices].tolist()
    tmp = non_edge_indices[non_edge_indices]
    tmp[[edge_index in polygon_last_edges
         for edge_index in edge_indices_list]] = False
    non_edge_indices[non_edge_indices] = tmp

    # prepare for rtree query
    query_edges = edges[non_edge_indices]
    query_bboxes = np.sort(query_edges, axis=1).reshape(-1, 4)
    unique_query_bboxes, unique_index, unique_inverse_index = np.unique(
        query_bboxes, return_index=True, return_inverse=True, axis=0)
    unique_query_edges = query_edges[unique_index]

    query_results = np.array([
        edge_intersect(key_edge, poly_edges[query_indices])
        for (key_edge, query_indices) in zip(unique_query_edges, [
            list(poly_rtree.intersection(bbox)) for bbox in unique_query_bboxes
        ])
    ])

    auxiliary_edges = edge_indices[non_edge_indices][unique_index][np.invert(
        query_results)]

    return np.vstack(poly_edge_indices), auxiliary_edges


def delaunay_triangulation(exterior_polygons, interior_polygons):
    tri = Delaunay(np.vstack(exterior_polygons + interior_polygons))

    # triangle is convex
    barycenters_clipper = np.sum(
        np.array(scale_to_clipper(tri.points))[tri.simplices], axis=1) / 3

    def barycenters_in_polygons(polygons):
        return [
            PointsInPolygon(barycenters_clipper, scale_to_clipper(polygon))
            for polygon in polygons
        ]

    points_in_exterior = np.any(np.array(
        barycenters_in_polygons(exterior_polygons)) == 1,
                                axis=0)
    point_not_in_interior = np.all(np.array(
        barycenters_in_polygons(interior_polygons)) == 0,
                                   axis=0)

    if not exterior_polygons:
        points_in_exterior = [True] * len(points_in_exterior)

    if not interior_polygons:
        point_not_in_interior = [True] * len(points_in_exterior)

    valid_indices = np.all((points_in_exterior, point_not_in_interior), axis=0)

    if not exterior_polygons or not interior_polygons:
        tmp = valid_indices[valid_indices]
        tmp[filter_intersecting_triangles(
            tri.points, tri.simplices[valid_indices],
            exterior_polygons + interior_polygons)] = False

        valid_indices[valid_indices] = tmp

        return tri.points, tri.simplices[valid_indices].tolist()

    else:
        poly_edges, auxiliary_edges = filter_auxiliary_edges(
            tri.points, tri.simplices[valid_indices],
            exterior_polygons + interior_polygons)

        all_edges = np.vstack([poly_edges, auxiliary_edges])
        adjaceny_lists = [
            adjaceny_list(all_edges, i) for i in range(len(tri.points))
        ]
        return tri.points, find_all_chordless_cycles(tri.points,
                                                     adjaceny_lists,
                                                     poly_edges)
```

> Patch for pyclipper

```
diff --git a/src/pyclipper/_pyclipper.pyx b/src/pyclipper/_pyclipper.pyx
index bb179a1..a952a01 100644
--- a/src/pyclipper/_pyclipper.pyx
+++ b/src/pyclipper/_pyclipper.pyx
@@ -319,6 +319,33 @@ def PointInPolygon(point, poly):
     return result
 
 
+def PointsInPolygon(points, poly):
+    """ Determine where does the point lie regarding the provided polygon.
+    More info: http://www.angusj.com/delphi/clipper/documentation/Docs/Units/ClipperLib/Functions/PointInPolygon.htm
+
+    Keyword arguments:
+    points -- a list of positions in question
+    poly  -- closed polygon
+
+    Returns:
+    List of array the same length as points, with each of its element
+        0  -- point is not in polygon
+        -1 -- point is on polygon
+        1  -- point is in polygon
+    """
+
+    cdef Path c_path = _to_clipper_path(poly)
+    cdef IntPoint *c_points = <IntPoint *> malloc(len(points) * sizeof(IntPoint))
+    cdef int* results = <int *> malloc(len(points) * sizeof(int))
+    for i in range(len(points)):
+        c_points[i] = _to_clipper_point(points[i])
+        with nogil:
+            results[i] = <int>c_PointInPolygon(c_points[i], c_path)
+    results_python = [results[i] for i in range(len(points))]
+    free(c_points)
+    return results_python
+
+
 def SimplifyPolygon(poly, PolyFillType fill_type=pftEvenOdd):
     """ Removes self-intersections from the supplied polygon.
     More info: http://www.angusj.com/delphi/clipper/documentation/Docs/Units/ClipperLib/Functions/SimplifyPolygon.htm
```