---
attachments: [affine_2d.ipynb]
tags: [Matrix Transformation]
title: Affine Transformation Matrix
created: '2021-12-11T08:54:10.367Z'
modified: '2021-12-11T10:36:02.990Z'
---

# Affine Transformation Matrix

Assume affine transformation matrix $\bm{M} = \begin{bmatrix} \bm{A} & \bm{b} \\ 0 & 1 \end{bmatrix} \in \mathbb{R}^{4 \times 4}$, where $\bm{A} = \left[\begin{array}{c|c|c} &  &  \\ \bm{x} & \bm{y} & \bm{z} \\&  & \end{array}\right] = \left[\begin{array}{c|c|c} x_{0} & y_{0} & z_{0} \\ x_{1} & y_{1} & z_{1} \\ x_{2} & y_{2} & z_{2} \end{array}\right]$, $\bm{b} = \begin{bmatrix} w_{0} \\ w_{1} \\ w_{2}  \end{bmatrix}$. 
$\frac{\bm{x}}{\lVert \bm{x} \rVert}$, $\frac{\bm{y}}{\lVert \bm{y} \rVert}$, $\frac{\bm{z}}{\lVert \bm{z} \rVert}$ define the new coordinate basis, $\lVert \bm{x} \rVert$, $\lVert \bm{y} \rVert$, $\lVert \bm{z} \rVert$ define scaling along basis and $\bm{b}$ defines the new origin.

Assume vector $\bm{v^{'}} = \begin{bmatrix} \bm{v} & 1 \end{bmatrix}$, where $\bm{v} = \begin{bmatrix} v_0 & v_1 & v_2 \end{bmatrix}$ , $\bm{M} \cdot \bm{v^{'}} = \bm{A} \cdot \bm{v} + \bm{b}$
### Column major multiplication
$\bm{A} \cdot \bm{v} = \left[\begin{array}{c|c|c} x_0 & y_0 & z_0 \\ x_1 & y_1 & z_1 \\ x_2 & y_2 & z_2 \end{array}\right] \begin{bmatrix} v_0 \\ v_1 \\ v_2 \end{bmatrix} = \begin{bmatrix} x_0 v_0 + y_0 v_1 + z_0 v_2 \\ x_1 v_0 + y_1 v_1 + z_1 v_2 \\ x_2 v_0 + y_2 v_1 + z_2 v_2 \end{bmatrix}$

### Row major multiplication
$\bm{v} \cdot \bm{A} = \begin{bmatrix} v_0 & v_1 & v_2 \end{bmatrix} \left[\begin{array}{ccc} x_0 & x_1 & x_2 \\ \hline y_0 & y_1 & y_2 \\ \hline z_0 & z_1 & z_2 \end{array}\right] = \begin{bmatrix} x_0 v_0 + y_0 v_1 + z_0 v_2 & x_1 v_0 + y_1 v_1 + z_1 v_2 & x_2 v_0 + y_2 v_1 + z_2 v_2 \end{bmatrix}$

### Storage
Usually (like in openGL) contiguous in memory: $\begin{bmatrix} x_{0} & x_{1} & x_{2} & 0 & y_{0} & y_{1} & y_{2} & 0 & z_{0} & z_{1} & z_{2} & 0 & w_{0} & w_{1} & w_{2} & 1 \end{bmatrix}$. In practice, the multiplication is equivalent to $v_0\begin{bmatrix} x_0 & x_1 & x_2 \end{bmatrix} + v_1\begin{bmatrix} y_0 & y_1 & y_2 \end{bmatrix} + v_2\begin{bmatrix} z_0 & z_1 & z_2 \end{bmatrix}$

### Minor, cofactor, adjoint
- Minor $\bm{M}_{ij}$ of matrix $\bm{A}$ is the determinant of submatrix of $\bm{A}$ by deleting row $i$ column $j$
- Cofactor $\bm{c}_{ij} = {(-1)}^{i+j}\bm{M}_{ij}$
- Cofactor matrix $\bm{C} = \begin{bmatrix} c_{00} & c_{01} & \dots \\ \vdots & \ddots & \\ c_{n0} & & c_{cc} \end{bmatrix}$
- Adjoint matrix $adj(\bm{A}) = \bm{C}^{T}$
- Inverse $\bm{A}^{-1} = \frac{adj(\bm{A})}{det(\bm{A})}$

### Chain multiplication
- Transformation of coordinate system: $\bm{T_{cb}} \bm{T_{ba}}$ transform from $a$ to $b$ to $c$, $\bm{{T_{ba}}}^{-1}\bm{T_{b}}\bm{T_{ba}}$ transform from $a$ to $b$ then back to $a$
- Non-uniform scale: The upper left submatrix is no longer guaranteed to be orthogonal, so $\left[ \frac{\bm{x}}{\lVert \bm{x} \rVert}^{T} \frac{\bm{y}}{\lVert \bm{y} \rVert}^{T} \frac{\bm{z}}{\lVert \bm{z} \rVert}^{T} \right] \not \in SO3$

### Transform normal vector
$\bm{n}^{*} = \bm{M}^{-T} \bm{n}$
#### proof
Assume $\bm{v}$ is a vector on surface, $\bm{n}$ is the surface normal vector, it satisfied $\bm{n}^{T} \bm{v} = 0$.
$$\bm{n}^{T} \bm{M}^{-1} \bm{M} \bm{v} = 0$$
$$(\bm{M}^{-T} \bm{n})^{T} \bm{M} \bm{v} = 0$$
Let the newly transformed vector on surface be $\bm{v}^{'} = \bm{M} \bm{v}$
$$(\bm{M}^{-T} \bm{n})^{T} \bm{v}^{'} = 0$$
So the transformed surface norm is $\bm{M}^{-T} \bm{n}$
