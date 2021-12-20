---
tags: [Matrix Transformation]
title: Rotation from to
created: '2021-12-20T03:22:37.768Z'
modified: '2021-12-20T03:23:14.331Z'
---

# Rotation from to

https://math.stackexchange.com/questions/180418/calculate-rotation-matrix-to-align-vector-a-to-vector-b-in-3d/897677#897677
```
def rotation_from_vectors(from_vector: glm.vec3, to_vector: glm.vec3):
    a = from_vector
    b = to_vector

    cos = glm.dot(a, b)
    cross = glm.cross(b, a)
    sin = glm.l2Norm(cross)
    proj = a
    reject = glm.normalize(b - cos * a)
    
    G = glm.mat3((cos, sin, 0), (-sin, cos, 0), (0, 0, 1))
    F = glm.mat3(proj, reject, glm.normalize(cross))
    
    return  F * G * glm.transpose(F)
```
