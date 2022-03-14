---
id: dJLXTtYPWBZwj5Gd8oOY6
title: Chain Multiplication
desc: ''
updated: 1646976471591
created: 1640018271719
---

[Jupyer demo](/assets/documents/affine_2d.ipynb)

## Transformation of coordinate system
$\bm{T_{cb}} \bm{T_{ba}}$ transform from $a$ to $b$ to $c$, $\bm{{T_{ba}}}^{-1}\bm{T_{b}}\bm{T_{ba}}$ transform from $a$ to $b$ then back to $a$

## Non-uniform scale
The upper left submatrix is no longer guaranteed to be orthogonal (has non-orthonormal basis), so $\left[ \frac{\bm{x}}{\lVert \bm{x} \rVert}^{T} \frac{\bm{y}}{\lVert \bm{y} \rVert}^{T} \frac{\bm{z}}{\lVert \bm{z} \rVert}^{T} \right] \notin SO3$

