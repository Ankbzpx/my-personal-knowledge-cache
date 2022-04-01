---
id: B24gWaRhhXusMzJNxWLtZ
title: Affine Transformation
desc: ''
updated: 1647498002088
created: 1640017905620
---

## Definition

### Linear transformation

Linear function, transform input vector to output vector

- Preserves parallelism for all lines (grid line parallel evenly spaced)
- Origin remain fixed

### Affine transformation
Linear transformation + translation

> No longer perserves origin

> Not necessarily perserves distances and angles

## Properity

Assume affine transformation matrix $\bm{M} = \begin{bmatrix} \bm{A} & \bm{b} \\ 0 & 1 \end{bmatrix} \in \mathbb{R}^{4 \times 4}$, where $\bm{A} = \left[\begin{array}{c|c|c} &  &  \\ \bm{x} & \bm{y} & \bm{z} \\&  & \end{array}\right] = \left[\begin{array}{c|c|c} x_{0} & y_{0} & z_{0} \\ x_{1} & y_{1} & z_{1} \\ x_{2} & y_{2} & z_{2} \end{array}\right]$, $\bm{b} = \begin{bmatrix} w_{0} \\ w_{1} \\ w_{2}  \end{bmatrix}$. 
$\frac{\bm{x}}{\lVert \bm{x} \rVert}$, $\frac{\bm{y}}{\lVert \bm{y} \rVert}$, $\frac{\bm{z}}{\lVert \bm{z} \rVert}$ define the new coordinate basis, $\lVert \bm{x} \rVert$, $\lVert \bm{y} \rVert$, $\lVert \bm{z} \rVert$ define scaling along basis and $\bm{b}$ defines the new origin.

Assume vector $\bm{v^{'}} = \begin{bmatrix} \bm{v} & 1 \end{bmatrix}$, where $\bm{v} = \begin{bmatrix} v_0 & v_1 & v_2 \end{bmatrix}$ , $\bm{M} \cdot \bm{v^{'}} = \bm{A} \cdot \bm{v} + \bm{b}$
