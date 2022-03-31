---
id: VNmhNJO2t34DeIBysB4wS
title: Skinning
desc: ''
updated: 1641299798619
created: 1640595277441
---

## Skinned mesh data structure
```
Node {
    skin: Skin
    mesh: Mesh
    // transformation defined by matrix or rotation/scale/translation
    matrix: mat4
    rotation: vec4
    scale: vec3
    translation: vec3
}

Skin {
    // len(inverseBindMatrices) == len(joints)
    inverseBindMatrices: [mat4f]
    joints: [int]
    ...
}

Mesh {
    primitives: [Primitive]
    ...
}

Primitive {
    attributes {
        POSITION: [vec3]
        NORMAL: [vec3]
        // len(JOINTS_n) == len(WEIGHTS_n), usually 4
        JOINTS_n: [int]
        WEIGHTS_n: [float]
        ...
    }
    indices: [int]
    ...
}

```

## Skinning matrix

### Joint matrix $T_{Joint}$
- $T_{WorldMesh}$: Mesh global transformation matrix
- $T_{WorldJoint}$: Joint global transformation matrix
- $T_{JointMesh}$: Inverse bind matrix

$$
T_{Joint} = {T_{WorldMesh}}^{-1} T_{WorldJoint} T_{JointMesh}
$$

Global transformation can be computed via chain multiplication. $T_{Joint}$ computation involves the transformation of coordinate system.
$T_{mesh}$ -> $T_{joint}$ -> $T_{mesh}$. See: [[Transformation of coordinate system|linear-algebra.affine-transformation.multiplication.chain-multiplication#transformation-of-coordinate-system]]

### Skin Matrix $T_{Skin}$
- $T_{Joint\ i}$: Joint matrix for joint i
- $W_{Joint\ i}$: Weight for Joint i

$$
T_{Skin} = W_{Joint\ 0} T_{Joint\ 0} + W_{Joint\ 1} T_{Joint\ 1} + \ldots + W_{Joint\ n} T_{Joint\ n}
$$

$T_{Skin}$ is in mesh/vert coordinate, per primitive.
