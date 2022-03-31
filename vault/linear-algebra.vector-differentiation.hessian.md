---
id: s73xtjl3w5rm6ph172yvjxd
title: Hessian
desc: ''
updated: 1648695925610
created: 1648695925610
---

> Reference: https://www.deeplearningbook.org/contents/numerical.html

Measure of curvature

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

$H$ is symmetric, a.k.a. $H_{ij} = H_{ji}$, wherever the second order partial derivatives are continuous. Thus, it can be decomposed into orthogonal eigen basis

> Hessian is Jacobian of Jacobian, **NOT** the dot product of Jacobian

For $f: \mathbb{R}^{n} \rarr \mathbb{R}^{m}$, $\pmb{x} \in \mathbb{R}^{n}$, $\pmb{f}(\pmb{x}) \in \mathbb{R}^{m}$, $\bm{H} \in \mathbb{R}^{m \times n \times n}$
