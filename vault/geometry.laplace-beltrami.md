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

### Isometry
Distance perserving mapping

### Divergence of gradient
Gradient as a function of vector field
- Maxima becomes sinks, minima becomes sources
- Divergence measure how much it locally behaves like a sink/source

Divergence operator $\nabla \cdot$ maps vector to scalar $\mathbb{R}^n \to \mathbb{R}$

> Is it equivalent to the sum of partial derivatives

### Laplace equation

$$
\Delta u = 0
$$

$u = g$ on boundary

### Poisson equation

$$
\Delta u = -f
$$

$u = g$ on boundary

### Riemannian manifold
Real smooth manifold with positive-definite inner product on tangent space for each point

### Riemannian Metric
Riemannian Metric is the collection of all inner products in Riemannian manifold

$$
g : T_pM \times T_pM \to \mathbb{R}
$$

Maps two tangent vectors to a real number (inner product)

### Random walk
Average of many random walks -> time varying gaussian (heat kernel)

Evaluation at time t -> integration of domain at time t -> expected value at time t

Laplacian -> derivative of expected vvalue over time

## Motive
- Reduce problem to solve sparse linear equation
- Describe **Mean curvature**
- **Isometry invariance**
Property preserves if transformation doesn't change point to point distance
- **Self-adjoint** (behave like symmetric)
- Positive definite (behave like definite), convex, unique minimizer
- Allow **Frequency decomposition**, fourier basis

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

## Fitting interpolating function
Fit function to interpolate missing data

Matching boundary + close to constant -> minimizing Dirichlet energy

$$
\nabla \bm{E}[u] = 0
$$

Minimizing Dirichlet energy is equivalent of solving Laplace equation

> Need more info

### Dirichlet energy
> Reference: https://math.stackexchange.com/questions/3213598/what-are-the-use-cases-of-the-dirichlet-energy-in-computer-vision

Measurement of how function change over some region $\Omega$ (smoothness, 0 for constant)

$$
\bm{E}[u] = \frac{1}{2} \int_{\Omega} \| \nabla u(x) \|_2^2 dx
$$

$\bm{E}[u]$ is convex

Fixed boundary condition (Dirichlet)

### Harmonic Function
$$
\Delta u = 0
$$

- Mean value property
- Min/max must be found on boundary

## boundary condition
Solution may not always exist for given boundary condition
- Dirchlet boundary: fixed value
- Neumann boundary: fixed derivative
- Mixed: fixed value + fixed derivative

## TODO:
- [ ] Divergence of gradient
- [ ] Divergence theorem
- [x] Trace of Hessian
- [x] Directional derivative
- [x] Hessian in bilinear form
- [ ] Brownian motion (Randon walk)
- [ ] Laplace equation, poisson equation
- [ ] Riemannian Metric
- [ ] Dirichlet energy
- [ ] Exterior derivative
- [ ] Covariant derivative
- [ ] Self-adjoint
- [ ] Spectral theorem
- [ ] Harmonic function
- [ ] Curvature, mean curvature
- [ ] Discrete Laplacian