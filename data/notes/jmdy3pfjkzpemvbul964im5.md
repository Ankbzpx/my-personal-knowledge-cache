
> Reference: https://www.youtube.com/watch?v=oEq9ROl9Umk

## Concepts

### Isometry
> Reference: https://mathworld.wolfram.com/Isometry.html

Distance perserving mapping

$$
d(f(x), f(y)) = d(x, y)
$$

2d case: rigid transformation + reflection

#### Rigid transformation
rotation + translation

### Divergency theorem
> Reference: https://www.youtube.com/watch?v=rB83DpBJQsE

#### Vector field
Associate each point on space with a vector

#### Divergence
Measure how much it locally behaves like a sink/source 

##### 2D case
how much $(x, y)$ generate fluid

$$
F(x, y) = F_x \bm{i} + F_y \bm{j}
$$

$$
\nabla \cdot F(x, y) = \frac{\partial F_x}{\partial x} + \frac{\partial F_y}{\partial y}
$$

> **NOT** equivalent to the sum of partial derivatives

Divergence operator $\nabla \cdot$ maps vector to scalar $\mathbb{R}^n \to \mathbb{R}$

#### Incompressible
Like fluid flow, no source, no sink
$$
\nabla \cdot F = 0
$$

#### Divergence of gradient
Gradient as a function of vector field, maxima becomes sinks, minima becomes sources

#### Divergence Theorem
> Reference: https://mathworld.wolfram.com/DivergenceTheorem.html

For a volume V with boundary $\partial V$

$$
\int_V \nabla \cdot F dV = \int_{\partial V} F da
$$

The volume intergration of divergence is equivalent to the surface integration of volume boundary.

It means the density within the volume can only be changed by flowing in/out through boundary

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
1. Average of many random walks -> time varying gaussian distributed (heat kernel)
2. Evaluation at time t -> integration of domain at time t -> expected value at time t
3. Laplacian -> derivative of expected value over time

### Dirichlet energy
> Reference: https://math.stackexchange.com/questions/3213598/what-are-the-use-cases-of-the-dirichlet-energy-in-computer-vision

Measurement of how function change over some region $\Omega$ (smoothness, 0 for constant)

$$
E(u) = \frac{1}{2} \int_{\Omega} \| \nabla u(x) \|_2^2 dx
$$

$E(u)$ is convex

Dirichlet boundary means fixed boundary condition

### Harmonic Function
$$
\Delta u = 0
$$

- Mean value property
- Min/max must be found on boundary

## Motive
- Reduce problem to solve sparse linear equation
- Describe **Mean curvature**
- **Isometry invariance**
Property preserves if transformation doesn't change point to point distance
- **Self-adjoint** (behave like symmetric)
- Positive definite, convex, unique minimizer
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

#### Deivation from local average view

##### Discrete
Given graph $G=(V,E)$, with value $u_i$ at each vertex i

Graph Laplacian $L$ gives deviation from average value of all neighbours $j$

$$
(Lu)_i := (\frac{1}{deg(i)}\sum_{ij \in E}u_j) - u_i
$$

- $deg$ means [[Valence|geometry.subdivision#valence]] of vertex $i$

##### Continuous
Difference between the value at point $x_0$ and the average value over a small sphere centered at $x_0$

$$
\Delta u(x_0) \propto \lim_{\epsilon \to 0} \frac{1}{\epsilon^2}(\frac{1}{|S_{\epsilon}(x_0)|} \int_{S_{\epsilon}(x_0)} u(x)dx - u(x_0))
$$

- $\epsilon$ is the radius of sphere
- $|S_{\epsilon}(x_0)|$ is the area of the sphere
- $\int_{S_{\epsilon}(x_0)} u(x)dx$ is the integration of value over sphere
- $u(x_0)$ value at center

## Data fitting (interpolating)
Fit function to interpolate missing data

Given:
- region $\Omega \in \mathbb{R}^2$
- boundary values $g:\partial \Omega \to \mathbb{R}$

Find function $u$:
- equal to boundary
- fill interior as smooth (close to constant) as possible

Minimizing [[Dirichlet energy|geometry.laplacian#dirichlet-energy]]
$$
\min_{u:\Omega \to \mathbb{R}} E (u) \\
s.t. u = g|_{\partial \Omega}
$$


Minimizing Dirichlet energy is equivalent of solving Laplace equation

$$
\begin{aligned}
\Delta u &= 0 \ on \ \Omega \\
u &= g \ on \ \partial \Omega
\end{aligned}
$$

## boundary condition
Solution may not always exist for given boundary condition
- Dirchlet boundary: fixed value
- Neumann boundary: fixed derivative
- Mixed: fixed value + fixed derivative

## TODO:
- [x] Divergence of gradient
- [x] Divergence theorem
- [x] Trace of Hessian
- [x] Directional derivative
- [x] Hessian in bilinear form
- [x] Brownian motion (Randon walk)
- [x] Laplace equation, poisson equation
- [x] Riemannian Metric
- [x] Dirichlet energy
- [ ] Minimize Dirichlet energy equals Laplace proof
- [ ] Exterior derivative
- [ ] Covariant derivative
- [ ] Self-adjoint
- [ ] Spectral theorem
- [ ] Harmonic function
- [ ] Curvature, mean curvature
- [ ] Discrete Laplacian
