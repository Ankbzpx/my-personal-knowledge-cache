---
id: s73xtjl3w5rm6ph172yvjxd
title: Hessian
desc: ''
updated: 1648695925610
created: 1648695925610
---

> Reference: https://www.deeplearningbook.org/contents/numerical.html

## Second order derivative

Second order derivative determines the quadratic term in function [[Local approximation|linear-algebra.vector-differentiation.taylor-series#local-approximation]], it is the measurement of curvature (convexity)
- $f'' = 0$ : Flat
- $f'' > 0$ : Convex
- $f'' < 0$ : Concave
- $\|f''\|$ : Determine curvature

## Matrix form definition

Let $f: \mathbb{R}^{n} \rarr \mathbb{R}$, $\pmb{x} \in \mathbb{R}^{n}$, $f(\pmb{x}) \in \mathbb{R}$:

$$
\bm{H} =
\bm{J}(\nabla_{\bm{x}} f)^T =
\begin{bmatrix}
\frac{\partial^2 f}{\partial x_1^2} & \frac{\partial^2 f}{\partial x_1 x_2} & \dots & \frac{\partial^2 f}{\partial x_1 x_n} \\
\frac{\partial^2 f}{\partial x_2 x_1} & \frac{\partial^2 f}{\partial x_2^2} & \dots & \frac{\partial^2 f}{\partial x_2 x_n} \\
\vdots &&& \vdots \\
\frac{\partial^2 f}{\partial x_n x_1} & \frac{\partial^2 f}{\partial x_n x_2} & \dots & \frac{\partial^2 f}{\partial x_n^2}
\end{bmatrix} \in \mathbb{R}^{n \times n}
$$


> Hessian is Jacobian of Jacobian, **NOT** the dot product of Jacobian

Let $f: \mathbb{R}^{n} \rarr \mathbb{R}^{m}$, $\pmb{x} \in \mathbb{R}^{n}$, $\pmb{f}(\pmb{x}) \in \mathbb{R}^{m}$

$$\bm{H} \in \mathbb{R}^{m \times n \times n}$$

## Directional derivative view (bilinear form)

Consider $f: \mathbb{R}^{n} \rarr \mathbb{R}$, $\pmb{x} \in \mathbb{R}^{n}$, $f(\pmb{x}) \in \mathbb{R}$, $\bm{H} \in \mathbb{R}^{n \times n}$

$\bm{H}$ is symmetric, a.k.a. $\bm{H}_{ij} = \bm{H}_{ji}$, wherever the second order partial derivatives are continuous. Thus, it can be decomposed into orthogonal eigen basis.

$$
\bm{H} = \bm{Q} \bm{\Lambda} \bm{Q}^T \\

\bm{\Lambda} = \bm{Q}^T \bm{H} \bm{Q}
$$

> More see [[linear-algebra.decomposition.eigendecomposition]] and [[Eigenbasis|linear-algebra.eigenvector-eigenvalue#eigenbasis]]

The second order [[linear-algebra.vector-differentiation.directional-derivative]] of direction vector $\bm{d}$ is given by

$$
\bm{d}^T \bm{H} \bm{d}
$$

If $\bm{d}$ is eigenvector, the result is its corresponding eigenvalue.