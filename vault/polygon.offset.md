---
id: zbxdptxtbmmvoecg0ub5eyd
title: Offset
desc: ''
updated: 1646726698246
created: 1646726636848
---

## Use library
[Clipper library](http://www.angusj.com/delphi/clipper.php)
([python binding](https://github.com/fonttools/pyclipper))

> Clipper seems to return a densely sampled polygon

## Using GLU
> Algorithm reference: https://mcmains.me.berkeley.edu/pubs/DAC05OffsetPolygon.pdf

> GLU reference chapter: https://people.eecs.ku.edu/~jrmiller/Courses/672/InClass/PolygonTessellation/PolygonTessellation.html

My implementation
```
# Only handles single polygon offset
# Algorithm: https://mcmains.me.berkeley.edu/pubs/DAC05OffsetPolygon.pdf
def polygon_edge_segments_offset(polygon, offset):
    vertex_start = polygon

    normalized = lambda x: x / (np.linalg.norm(x, axis=1) + 1e-10).reshape(
        -1, 1)

    def compute_edge_specs(edge_vector):
        edge_vector = np.insert(edge_vector, obj=2, values=0, axis=1)
        edge_vector_next = np.roll(edge_vector, -1, axis=0)
        # angle between two adjacent edges
        angles = np.arccos(-np.sum(edge_vector * edge_vector_next, axis=1) /
                           (np.linalg.norm(edge_vector, axis=1) *
                            np.linalg.norm(edge_vector_next, axis=1)) + 1e-10)
        # angle for arcs centered at shared vertices
        arc_angle = np.pi - angles
        # CCW, right concave, left convex
        is_convex = np.sign(np.cross(edge_vector, edge_vector_next)[:, -1]) > 0
        # CCW, toward right
        exterior_normal = np.cross(
            edge_vector,
            np.tile(np.array([0, 0, 1]), reps=(len(edge_vector), 1)))[:, :2]
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

    offset_edge_segments = edge_segments + (offset * exterior_normal).reshape(
        -1, 1, 2)
    offset_vertex_start = offset_edge_segments[:, 0, :]
    offset_vertex_end = offset_edge_segments[:, 1, :]
    offset_vertex_start_next = np.roll(offset_vertex_start, -1, axis=0)

    def subdivision(left, right, centroid, radius, level):
        if level == 0:
            return []
        middle = radius * normalized(0.5 *
                                     (left + right) - centroid) + centroid
        return subdivision(left, middle, centroid, radius, level - 1) + [
            middle
        ] + subdivision(middle, right, centroid, radius, level - 1)

    arc_vertices = np.array(
        subdivision(offset_vertex_end, offset_vertex_start_next, vertex_end,
                    abs(offset), 2)).transpose([1, 0, 2])

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
    auxiliary_vertices = [
        get_auxiliary_vertices(vertex_end[i], arc_vertices[i], subdivide[i])
        for i in range(len(subdivide))
    ]

    # edge vertices -> auxiliary vertices -> next edge vertices ...
    new_vertex_list = [[]] * 2 * len(subdivide)
    new_vertex_list[0::2] = lift_twice(offset_edge_segments)
    new_vertex_list[1::2] = auxiliary_vertices

    # Reference: https://stackoverflow.com/questions/952914/how-to-make-a-flat-list-out-of-a-list-of-lists
    new_vertex_list_merged = list(
        itertools.chain.from_iterable(new_vertex_list))

    return new_vertex_list, new_vertex_list_merged


# Reference: https://www.glprogramming.com/red/chapter11.html
def get_positive_winding_polygon_boundary(polygon):

    polygon = np.array(polygon)
    if polygon.shape[1] == 2:
        polygon = np.insert(polygon, 2, 0, axis=1)

    boundary_polygons = []

    def glu_tess_begin_callback(type):
        global boundary_polygon
        boundary_polygon = []

    def glu_tess_vertex_callback(vertex_data):
        global boundary_polygon
        boundary_polygon.append(vertex_data[:2])

    def glu_tess_end_callback():
        global boundary_polygon
        boundary_polygons.append(boundary_polygon)

    def glu_tess_error_callback(errno):
        print("glu_tess_error_callback", GLU.gluErrorString(errno))

    def glu_tess_combine_callback(vertex,
                                  neighbors,
                                  neighborWeights,
                                  out=None):
        out = vertex[:3]
        return out

    tes = GLU.gluNewTess()
    GLU.gluTessCallback(tes, GLU.GLU_TESS_BEGIN, glu_tess_begin_callback)
    GLU.gluTessCallback(tes, GLU.GLU_TESS_VERTEX, glu_tess_vertex_callback)
    GLU.gluTessCallback(tes, GLU.GLU_TESS_END, glu_tess_end_callback)
    GLU.gluTessCallback(tes, GLU.GLU_TESS_ERROR, glu_tess_error_callback)
    GLU.gluTessCallback(tes, GLU.GLU_TESS_COMBINE, glu_tess_combine_callback)
    GLU.gluTessProperty(tes, GLU.GLU_TESS_BOUNDARY_ONLY, GL.GL_TRUE)
    GLU.gluTessProperty(tes, GLU.GLU_TESS_WINDING_RULE,
                        GLU.GLU_TESS_WINDING_POSITIVE)
    GLU.gluTessNormal(tes, 0, 0, 1)

    GLU.gluTessBeginPolygon(tes, None)
    GLU.gluTessBeginContour(tes)
    for vert in polygon:
        GLU.gluTessVertex(tes, vert, vert)
    GLU.gluTessEndContour(tes)
    GLU.gluTessEndPolygon(tes)
    GLU.gluDeleteTess(tes)

    return boundary_polygons


def polygon_offset(polygon, offset):
    _, edge_offset_polygon = polygon_edge_segments_offset(polygon, offset)
    return get_positive_winding_polygon_boundary(edge_offset_polygon)
```
