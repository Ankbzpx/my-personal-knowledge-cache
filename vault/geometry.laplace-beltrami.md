---
id: jmdy3pfjkzpemvbul964im5
title: Laplace Beltrami
desc: ''
updated: 1648542195222
created: 1648542195222
---

> Reference: https://www.youtube.com/watch?v=oEq9ROl9Umk

Generalization of Laplacian in Euclidean space to curve domain

## Concepts
### second order derivative
Tell function convexity
- $f'' > 0$ : Convex
- $f'' < 0$ : Concave
- Magnitude : Determine curvature if first-order derivative is 0

### Laplace equation

$$
\Delta u = 0
$$

### Poisson equation

$$
\Delta u = f
$$


### Riemannian manifold
Real smooth manifold with positive-definite inner product on tangent space for each point

### Riemannian Metric
Riemannian Metric is the collection of all inner products in Riemannian manifold

$$
g : T_pM \times T_pM \to \mathbb{R}
$$

Maps two tangent vectors to a real number (inner product)

### Dirichlet energy
> Reference: https://math.stackexchange.com/questions/3213598/what-are-the-use-cases-of-the-dirichlet-energy-in-computer-vision

Measurement of how function change over some region $\Omega$

$$
\bm{E}[u] = \frac{1}{2} \int_{\Omega} \| \nabla u(x) \|_2^2 dx
$$

Minimizing Dirichlet energy is equivalent of solving Laplace equation

## Motive
- Reduce problem to solve sparse linear equation

- Describe **Mean curvature**

- **Isometry invariance**
Property preserves if transformation doesn't change point to point distance

- Allow **Frequency decomposition**

## Definition
### Euclidean space
$u:\mathbb{R}^n \to \mathbb{R}$

Laplacian is the sum of the second order derivative evaluated at the target point

$$
\Delta u = \sum_{i=1}^{n} \frac{\partial^2}{\partial x_i^2} u
$$

Which is also the trace of [[linear-algebra.vector-differentiation.hessian]]

$$
\Delta u = tr(H)
$$

### Deivation from local average
#### Descrete
Given graph $G=(V,E)$, with value $u_i$ at each vertex i

Graph Laplacian $L$ gives deviation from average value of all neighbours $j$

$$
(Lu)_i := (\frac{1}{deg(i)}\sum_{ij \in E}u_j) - u_i
$$

- $deg$ means [[Valence|geometry.subdivision#valence]] of vertex $i$

#### Continuous
Difference between the value at point $x_0$ and the average value over a small sphere centered at $x_0$

$$
\Delta u(x_0) \propto \lim_{\epsilon \to 0} \frac{1}{\epsilon^2}(\frac{1}{|S_{\epsilon}(x_0)|} \int_{S_{\epsilon}(x_0)} u(x)dx - u(x_0))
$$

- $\epsilon$ is the radius of sphere
- $|S_{\epsilon}(x_0)|$ is the area of the sphere
- $\int_{S_{\epsilon}(x_0)} u(x)dx$ is the integration of value over sphere
- $u(x_0)$ value at center


## TODO:
- [ ] Divergence of gradient
- [x] Trace of Hessian
- [x] Directional derivative
- [ ] Hessian in bilinear form
- [ ] Brownian motion (Randon walk)
- [ ] Laplace equation, poisson equation
- [ ] Riemannian Metric
- [ ] Dirichlet energy
- [ ] Curvature, mean curvature
- [ ] Discrete Laplacian