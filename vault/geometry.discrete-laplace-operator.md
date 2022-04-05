---
id: f4aa3in8po9mfmjv6rnnx0k
title: Discrete Laplace Operator
desc: ''
updated: 1648786314576
created: 1648786314576
---

## Concepts
### Curvature
> Reference: https://mathworld.wolfram.com/Curvature.html

Measure local bending of the surface

$$
r d \phi = d s \\
\phi = \int_0^t s'(t) \kappa(t) dt \\
r = \frac{1}{|\kappa|}
$$

where $\kappa$ is curvature, $s$ is arc length (length along a curve), $\phi$ is tangential angle, $r$ is radius of curvature


For 2d curve $y = f(x)$, its curvature is defined by
$$

\kappa = \frac{\frac{\partial^2 f}{\partial x^2}}{(1 + (\frac{\partial f}{\partial x})^2)^{\frac{3}{2}}}
$$

### Obtuse triangle
Triangle with one angle bigger than $\pi/2$, so its circumcenter point is outside triangle

### Mass matrix

Discretization of inner-product (kinda like [[kernel|linear-algebra.feature-extraction.principle-component-analysis#kernel-trick]]?)

## Discrete differential geometry operators

> Reference: http://multires.caltech.edu/pubs/diffGeoOps.pdf

Surface $S$ embedded in $\mathbb{R}^3$ can be locally approximated by tangent plane, orthogonal to normal vector $n$.

### Normal curvature
For unit direction $\bm{e_{\theta}}$ in the tangent plane, starting from the point where the tangent plane is evaluated, the normal curvature $\kappa_N(\theta)$ is defined as the curvature of curve that wraps $\bm{e_{\theta}}$ onto $S$. (like how matrix exponetiation wraps vector on tangent plane to manifold)

### Principle curvatures
Two extremum [[Normal curvature|geometry.discrete-laplace-operator#normal-curvature]] $\kappa_1$ and $\kappa_2$ (2 cause $S$ is 2 dimensional manifold) with associated orthogonal direction $\bm{e_1}$ and $\bm{e_2}$.

### Mean curvature
average of [[Normal curvature|geometry.discrete-laplace-operator#normal-curvature]]
$$
\kappa_H = \frac{1}{2\pi}\int_{0}^{2\pi} \kappa^N(\theta)d\theta
$$
It can also be defined with [[Principle curvatures|geometry.discrete-laplace-operator#principle-curvatures]] as
$$
\kappa_H = (\kappa_1 + \kappa_2)/2
$$


### Gaussian curvature
product of the two [[Principle curvatures|geometry.discrete-laplace-operator#principle-curvatures]]

$$
\kappa_G = \kappa_1 \kappa_2
$$


Relation between surface area minimization and mean curvature flow

$$
2\kappa_H\bm{n} = \lim_{diameter(A) \to 0} \frac{\nabla A}{A}
$$

### Laplace-Beltrami
Maps point on surface to vector

For $\bm{p} \in S$
$$
\bm{K}(\bm{p}) = 2 \kappa_H(\bm{p})\bm{n}(\bm{p})
$$


Define properies of surface at each vertex as spatial average of 1-ring neighbourhood of this vertex

#### 1-ring neighbourhood boundary
For a given vertex, find all barycenter/circumcenter for all incident triangles, piecewisely connect centers and incident edge midpoints.

Note that 1-ring neighbourhood for each vertex won't overlaps

### Discrete Mean curvature Normal

$$
\bm{K}(\bm{x}_i) = \frac{1}{2 A_{Mixed}} \sum_{j \in N_1(i)}(\cot \alpha_{ij} + \cot \beta_{ij})(\bm{x}_i - \bm{x}_j)
$$

where $\alpha_{ij}$ and $\beta_{ij}$ are angles opposite to shared edge $(\bm{x}_i, \bm{x}_j)$, $N_1(i)$ is the set of 1-ring neighbour vertex, $A_{Mixed}$ is

$$
\begin{equation}
A_{Mixed} = 
\begin{cases}
A_{voronoi}   & \text{non-obtuse triangle}\\
\frac{1}{4} A & \text{non-obtuse angle of obtuse triangle}\\
\frac{1}{2} A & \text{obtuse angle of obtuse triangle}
\end{cases}
\end{equation}
$$

that can be stored in diagonal [[Mass matrix|geometry.discrete-laplace-operator#mass-matrix]]

### Discrete Gaussian curvature

$$
\kappa_G(\bm{x}_i) = (2\pi - \sum_{j=i}^{\#f} \theta_j)/A_{Mixed}
$$

where $\theta_j$ the angle at the $j$-th face at vertex $\bm{x}_i$ $\#f$ is number of faces around this vertex

### Discrete Principle curvatures

Use equation defined in [[Mean curvature|geometry.discrete-laplace-operator#mean-curvature]] and [[Gaussian curvature|geometry.discrete-laplace-operator#gaussian-curvature]]

$$
\kappa_1(\bm{x}_i) = \kappa_H(\bm{x}_i) + \sqrt{\Delta(\bm{x}_i)} \\
\kappa_2(\bm{x}_i) = \kappa_H(\bm{x}_i) - \sqrt{\Delta(\bm{x}_i)}
$$
with $\Delta(\bm{x}_i) = \kappa_H^2(\bm{x}_i) - \kappa_G(\bm{x}_i)$ and $\kappa_H(\bm{x}_i) = \frac{1}{2}\|\bm{K}(\bm{x}_i)\|$. $\Delta(\bm{x}_i)$ must be positive for numerical reason

### Pre-Vertex Principle curvatures

[[Discrete Mean curvature Normal|geometry.discrete-laplace-operator#discrete-mean-curvature-normal]] can be written in quadratic form

$$
\bm{K}(\bm{x}_i) = \sum_{j\in N_1(i)} w_{ij} \kappa_{i,j}^N
$$
where $w_{ij} = \frac{1}{A_{Mixed}} (\frac{1}{8}(\cot \alpha_j + \cot \beta_j)\|\bm{x}_i - \bm{x}_j\|^2)$, $\kappa_{i,j}^n = 2 \frac{(\bm{x}_i - \bm{x}_j)\cdot \bm{n}}{\|\bm{x}_i - \bm{x}_j\|^2}$, which defines the [[Normal curvature|geometry.discrete-laplace-operator#normal-curvature]] along edge $\bm{x}_i\bm{x}_j$

Define a symmetric curvature tensor $B = \begin{bmatrix} a & b \\ b & c \end{bmatrix}$ that can be used to compute [[Normal curvature|geometry.discrete-laplace-operator#normal-curvature]] in any directions of the tangent plane
(kinda like the [[linear-algebra.inner-product]])

The unit direction of the projection of edge $\bm{x}_i\bm{x}_j$ on tangent plane can be computed as
$$
\bm{d}_{i,j}^T B \bm{d}_{i,j} = \kappa_{i,j}^N
$$

$$
\bm{d}_{i,j} = \frac{(\bm{x}_j - \bm{x}_i) - ((\bm{x}_j - \bm{x}_i) \cdot \bm{n}) \bm{n}}{\|(\bm{x}_j - \bm{x}_i) - ((\bm{x}_j - \bm{x}_i) \cdot \bm{n}) \bm{n}\|}
$$

Quadratic fit $B$ with constraints

$$
a + b = 2\kappa_H \\
ac - b^2 = \kappa_G
$$

> why with those constraint?

Perform eigenanalysis on $B$ to find the two [[Principle curvatures|geometry.discrete-laplace-operator#principle-curvatures]] for given vertex

## TODO
- [ ] Express normal curvature in terms of principle curvature
- [ ] Euler-Lagrange equation
- [ ] Surface area minimization
- [ ] Image of gaussian map
- [ ] Conformal space parameters
- [x] Obtuse triangle
- [x] Mass matrix
- [x] [[Cotangent|code-read.igl.laplacian#cotangent]]
- [x] Curvature tensor
- [ ] Curvature tensor fit constraints

