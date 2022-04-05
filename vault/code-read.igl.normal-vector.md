---
id: edudwmtuufpqosp7ztuwddr
title: Normal Vector
desc: ''
updated: 1647583831269
created: 1646974566576
---


## Concepts
### Normal Vector
Normal Vector at a point is the cross product of two tangent vectors at that point.
### Tangent Vector
A vector tangents to a surface at a given point
### Degenerated Normal Vector
two tangent vectors are linear depend

## Implementations

### per_face_normals

```
V: Vertices (NV x 3)
F: Faces (NF x 3)

f = F[i] # for face i
v1 = V[f[1]] - V[f[0]]
v2 = V[f[2]] - V[f[0]]
fn = cross(v1, v2)
if fn is zero vector:
    return degenerated normal
else:
    return normalized fn
```

### per_vertex_normals

> Weighted sum of adjacent faces' normal, interpolated by vertex shader

```
Weight W: NF x 3
case uniform: 
    all 1
case area: 
    compute face areas `doublearea` (NF x 1) replicated to (NF x 3)
case internal_angle: 
    `internal_angle` angle of each face cornors (angle between edges that share the same vertex)

# theoritically loop through each vertex, compute its normal vn by
vn = sum([w[i] * fn for normal of faces thats shares v])
# implementation-wise it's more efficient to loop over faces

return normalized vn

```

> Interpolated vertex normals -> [[Smooth shading in Blender|coding.blender.python-api#mesh-ops]]

### per_corner_normals
>Weighted (by double area) sum of selected (by angle between two normal) incident face normal, interpolated by vertex shader
>
>See: [[vertex_triangle_adjacency Unrolled|code-read.igl.neighbourhood-connectivity#unrolled]]


```
FA: double face area
FN: face norm
VF[NI[F(i, j)] + k]: (kth incident face) face index that incident to the jth vertex of face i
FN.row(VF[NI[F(i, j)] + k]): normal of (kth incident face) face that incident to the jth vertex of face i
```

### Vertex Shader
> ViewerData

Vertex
```
meshgl.V_normals_vbo = data.V_normals.cast<float>();
```
Face or corner
```
meshgl.V_normals_vbo.resize(data.F.rows()*3,3);
for (unsigned i=0; i<data.F.rows();++i)
    for (unsigned j=0;j<3;++j)
        meshgl.V_normals_vbo.row(i*3+j) =
            per_corner_normals ?
            data.F_normals.row(i*3+j).cast<float>() :
            data.F_normals.row(i).cast<float>();
```


> MeshGL

```
bind_vertex_attrib_array(shader_mesh,"normal", vbo_V_normals, V_normals_vbo, dirty & MeshGL::DIRTY_NORMAL);
```

> MeshGL:: mesh_vertex_shader

```
in vec3 normal;
```

## TODO
- [ ] Laplace-Beltrami operator
- [ ] Cotangent stiffness matrix (Polygon Laplacian Made Simple)


