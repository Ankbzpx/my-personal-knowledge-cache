---
id: DXryQkH15go9fC8P3uCYQ
title: Polygon Operations
desc: ''
updated: 1645177171477
created: 1642932269870
---
> Workflow: Simplify -> Smooth + resample -> Simplify -> Offset

## Polygon simplify
[Ramer–Douglas–Peucker & Visvalingam-Whyatt](https://github.com/urschrei/simplification)

## Polygon offset

### Library
[Clipper library](http://www.angusj.com/delphi/clipper.php)
([python binding](https://github.com/fonttools/pyclipper))

### Algorithm
> Algorithm reference: https://mcmains.me.berkeley.edu/pubs/DAC05OffsetPolygon.pdf

> GLU reference chapter: https://people.eecs.ku.edu/~jrmiller/Courses/672/InClass/PolygonTessellation/PolygonTessellation.html

```
import numpy as np

# https://stackoverflow.com/questions/1165647/how-to-determine-if-a-list-of-polygon-points-are-in-clockwise-order
def is_CCW(coutour):
    start = coutour[1:]
    end = coutour[:-1]
    return np.sum((start[:, 0] - end[:, 0]) / (start[:, 1] + end[:, 1])) < 0

from OpenGL import GL, GLU

# Reference: https://www.glprogramming.com/red/chapter11.html
def get_positive_winding_polygon_boundary(polygon):
    boundary_polygons = []
    boundary_polygon = []

    def glu_tess_begin_callback(type):
        boundary_polygon = []

    def glu_tess_vertex_callback(vertex_data):
        boundary_polygon.append(vertex_data[:2])

    def glu_tess_end_callback():
        boundary_polygons.append(boundary_polygon)

    def glu_tess_error_callback(errno):
        print("glu_tess_error_callback", GLU.gluErrorString(errno))

    def glu_tess_combine_callback(vertex, neighbors, neighborWeights, out=None):
        out = vertex
        return out

    tes = GLU.gluNewTess()
    GLU.gluTessCallback(tes, GLU.GLU_TESS_BEGIN, glu_tess_begin_callback)
    GLU.gluTessCallback(tes, GLU.GLU_TESS_VERTEX, glu_tess_vertex_callback)
    GLU.gluTessCallback(tes, GLU.GLU_TESS_END, glu_tess_end_callback)
    GLU.gluTessCallback(tes, GLU.GLU_TESS_ERROR, glu_tess_error_callback)
    GLU.gluTessCallback(tes, GLU.GLU_TESS_COMBINE, glu_tess_combine_callback)
    GLU.gluTessProperty(tes, GLU.GLU_TESS_BOUNDARY_ONLY, GL.GL_TRUE)
    GLU.gluTessProperty(tes, GLU.GLU_TESS_WINDING_RULE, GLU.GLU_TESS_WINDING_POSITIVE)
    GLU.gluTessNormal(tes, 0.0, 0.0, 1.0 if is_CCW(polygon) else -1.0)

    GLU.gluTessBeginPolygon(tes, None)
    GLU.gluTessBeginContour(tes)
    for vert in polygon:
        GLU.gluTessVertex(tes, vert, vert)
    GLU.gluTessEndContour(tes)
    GLU.gluTessEndPolygon(tes)
    GLU.gluDeleteTess(tes)

    return boundary_polygons

import itertools

def polygon_edge_segments_offset(vertex_start, offset):

    normalized = lambda x: x / (np.linalg.norm(x, axis=1) + 1e-10).reshape(-1, 1)

    def compute_edge_specs(edge_vector):
        edge_vector = np.insert(edge_vector, obj=2, values=0, axis=1)
        edge_vector_next = np.roll(edge_vector, -1, axis=0)
        # angle between two adjacent edges
        angles = np.arccos(-np.sum(edge_vector * edge_vector_next, axis=1) / (np.linalg.norm(edge_vector, axis=1) * np.linalg.norm(edge_vector_next, axis=1)))
        # angle for arcs centered at shared vertices
        arc_angle = np.pi - angles
        # CCW, right concave, left convex
        is_convex = np.sign(np.cross(edge_vector, edge_vector_next)[:, -1]) > 0
        # CCW, toward right
        exterior_normal = np.cross(edge_vector, np.tile(np.array([0, 0, 1]), reps=(len(edge_vector), 1)))[:, :2]
        return arc_angle, is_convex, normalized(exterior_normal)

    vertex_end = np.roll(vertex_start, -1, axis=0)
    edge_segments = np.hstack((vertex_start, vertex_end)).reshape(-1, 2, 2)
    edge_vector = vertex_end - vertex_start
    arc_angle, is_convex, exterior_normal = compute_edge_specs(edge_vector)

    # Subdivide with a simple policy
    # Interval:    0   pi/8   3*pi/8
    # Subdivision:   |  0  |  2  |  4
    c1 = arc_angle > np.pi / 8
    c2 = arc_angle < 3 * np.pi / 8

    with_arc = is_convex if np.sign(offset) > 0 else np.invert(is_convex)
    query_d1 = np.all([np.all([c1, c2], axis=0), with_arc], axis=0)
    query_d2 = np.all([np.invert(c2), with_arc], axis=0)
    
    # -1: use shared vertex
    #  0: direct connect
    #  1: divide once, then connect
    #  2: divide twice, then connect
    subdivide = np.zeros(len(query_d2))
    subdivide[np.invert(with_arc)] = -1
    subdivide[query_d1] = 1
    subdivide[query_d2] = 2

    offset_edge_segments = edge_segments + (offset * exterior_normal).reshape(-1, 1, 2)
    offset_vertex_start = offset_edge_segments[:, 0, :]
    offset_vertex_end = offset_edge_segments[:, 1, :]
    offset_vertex_start_next = np.roll(offset_vertex_start, -1, axis=0)

    def subdivision(left, right, centroid, radius, level):
        if level == 0:
            return []
        middle = radius * normalized(0.5 * (left + right) - centroid) + centroid
        return subdivision(left, middle, centroid, radius, level-1) + [middle] + subdivision(middle, right, centroid, radius, level - 1)

    arc_vertices = np.array(subdivision(offset_vertex_end, offset_vertex_start_next, vertex_end, abs(offset), 2)).transpose([1, 0, 2])

    lift_once = lambda x: [y for y in x]
    lift_twice = lambda x: [lift_once(y) for y in x]

    def get_auxiliary_vertices(shared_vertices, auxiliary_vertices, level):
        if level == -1:
            return [shared_vertices]
        elif level == 0:
            return []
        elif level == 1:
            return [auxiliary_vertices[1]]
        elif level == 2:
            return lift_once(auxiliary_vertices)

    # no idea how to vectorize, use for loop instead
    auxiliary_vertices = [get_auxiliary_vertices(vertex_end[i],  arc_vertices[i], subdivide[i]) for i in range(len(subdivide))]

    # edge vertices -> auxiliary vertices -> next edge vertices ...
    new_vertex_list = [[]] * 2 * len(subdivide)
    new_vertex_list[0::2] = lift_twice(offset_edge_segments)
    new_vertex_list[1::2] = auxiliary_vertices

    # Reference: https://stackoverflow.com/questions/952914/how-to-make-a-flat-list-out-of-a-list-of-lists
    new_vertex_list_merged = list(itertools.chain.from_iterable(new_vertex_list))

    return new_vertex_list, new_vertex_list_merged

```

## Polygon smooth
[Bezier Curves Interpolation](https://web.archive.org/web/20131027060328/http://www.antigrain.com/research/bezier_interpolation/index.html#PAGE_BEZIER_INTERPOLATION)

My numpy implementation:
### Bezier fit
```
def bezier_fit(polygon, smooth_factor=1.0):
    """
    Bezier_curve_interpolation.
    
    Parameters
    ----------
    polygon : n x 2 numpy array
    smooth_factor : the larger the value, more close the curve is to the edge
    resample_count : number of samples for each bezier segment

    Returns
    -------
    control_points : bezier control points in the form of n x 3 x 2 numpy array (center, left_handle, right_handle)
    """

    polygon_next = np.roll(polygon, -1, axis=0)
    polygon_last = np.roll(polygon, 1, axis=0)
    mid_point = (polygon + polygon_next) / 2

    dist_to_last = np.linalg.norm(polygon - polygon_last, axis=1)
    dist_to_next = np.linalg.norm(polygon - polygon_next, axis=1)
    dist_sum = smooth_factor * (dist_to_next + dist_to_last)
    dir = np.roll(mid_point, 1, axis=0) - mid_point

    cpt_left = polygon + dir * (dist_to_last / dist_sum).reshape(-1, 1)
    cpt_right = polygon - dir * (dist_to_next / dist_sum).reshape(-1, 1)

    control_points = np.array(
        [polygon, cpt_left, cpt_right]).transpose([1, 0, 2])
    
    return control_points
```

### Bezier fit and resample
```
def bezier_curve_interpolation(polygon, smooth_factor = 1.0, resample_count = 8):
    """
    Bezier_curve_interpolation.
    
    Parameters
    ----------
    polygon : n x 2 numpy array
    smooth_factor : the larger the value, more close the curve is to the edge
    resample_count : number of samples for each bezier segment
    """

    def bezier_curve(cpts, t):
        tile = lambda x: np.tile(np.expand_dims(x, -1), len(t))
        pt0 = tile(cpts[0])
        pt1 = tile(cpts[1])
        pt2 = tile(cpts[2])
        pt3 = tile(cpts[3])
        return np.power(1 - t, 3) * pt0 + 3 * np.power(1 - t, 2) * t * pt1 + 3 * (1 - t) * np.power(t, 2) * pt2 + np.power(t, 3) * pt3
    
    polygon_next = np.roll(polygon, -1, axis=0)
    polygon_last = np.roll(polygon, 1, axis=0)
    mid_point = (polygon + polygon_next) / 2

    dist_to_last = np.linalg.norm(polygon - polygon_last, axis=1)
    dist_to_next = np.linalg.norm(polygon - polygon_next, axis=1)
    dist_sum = smooth_factor * (dist_to_next + dist_to_last)
    dir = np.roll(mid_point, 1, axis=0) - mid_point

    cpt_left = polygon + dir * (dist_to_last / dist_sum).reshape(-1, 1)
    cpt_right = polygon - dir * (dist_to_next / dist_sum).reshape(-1, 1)

    control_points = np.array([polygon, cpt_right, np.roll(cpt_left, -1, axis=0), polygon_next])
    return bezier_curve(control_points, np.linspace(0, 1, resample_count + 1)[:-1]).transpose((0, 2, 1)).reshape(-1, 2)
```
