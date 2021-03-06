---
id: qPFCR5icUF6If7yO8Ezdw
title: Rotation from Vector to Vector
desc: ''
updated: 1640018478523
created: 1640018469639
---

```
// Reference: https://math.stackexchange.com/a/897677
mat3f rotation(float3 fromVec, float3 toVec){
    float3 a = fromVec;
    float3 b = toVec;
    float dotProd = dot(a, b);
    float3 crossProd = cross(a, b);
    float cos = dotProd;
    float sin = norm(crossProd);
    // pure rotation matrix
    auto G = mat3f(float3(cos, sin, 0), float3(-sin, cos, 0), float3(0, 0, 1));
    // three orthogonal basis the rotation applied upon
    // 1. normalized vector projection of b on a
    // 2. normalized vector rejection of b on a
    // 3. cross product of b and a
    // the basis change matrix is actually F^-1
    auto F = mat3f(a, normalize(b - cos * a), normalize(crossProd));
    // orthonormal basis form orthogonal matrix
    mat3f R = F * G * transpose(F);
    return R;
}
```
