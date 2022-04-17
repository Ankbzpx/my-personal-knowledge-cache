---
id: 6jal7oacj8b4p1cg6aluj7v
title: Mesh Parameterization
desc: ''
updated: 1649993537447
created: 1649923330976
---

### boundary_loop
1. Compute [[vertex_triangle_adjacency|code-read.igl.neighbourhood-connectivity#vertex_triangle_adjacency]]
2. Compute [[triangle_triangle_adjacency|code-read.igl.mesh-parameterization#triangle_triangle_adjacency]]
3. Compute [[is_border_vertex|code-read.igl.mesh-parameterization#is_border_vertex]], create a set for all border vertices
4. Perform breadth-first-search to traverse border vertice till the edge loop is found. 

```
enqueue one border vertex
while not finished:
    dequeue latest border vertex
    for all its incident triangles:
        if the triangle contains boundary edge:
            enque the other vertex of the border edge
``` 

### triangle_triangle_adjacency
1. Compute [[vertex_triangle_adjacency|code-read.igl.neighbourhood-connectivity#vertex_triangle_adjacency]]
2. Compute adjacency matrix `TT` (face index -> index of adjacent face)
```
for each face for each corner vertex in face:
    vi: current vertex
    vin: next vertex in current face
    for all incident faces of vi:
        if incident face index does not equal to current face index:
            if that incident face contains vin:
                that face is adjacent to current face w.r.t. edge [vi vin] (share same edge)             
```
3. Compute adjacency matrix `TTi` (face index -> incident edge index in incident triangle)
```
for each face for each corner vertex in face:
    vi: current vertex
    vj: next vertex in current face
    fn: face adjacent to current face w.r.t. edge [vi vj]
    if fn exists:
        for each corner vertex of fn:
            vin: current vertex in fn
            vjn: next vertex in fn
            if edge [vjn, vin] is [vi vj]:
                record corner vertex index
```

### is_border_vertex
1. Compute [[triangle_triangle_adjacency|code-read.igl.mesh-parameterization#triangle_triangle_adjacency]]
2. Find edge of triangle that has not incident triangle, the two vertices of that edge are border vertices

### map_vertices_to_circle
1. Compute cumulated length for each boundary edges
2. Treat total length as circumference of a unit circle
3. Boundary uv coordinates are then $(cos, sin)$ of their portion w.r.t. circumference