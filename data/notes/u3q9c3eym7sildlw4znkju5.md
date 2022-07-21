
> Reference: https://graphics.stanford.edu/courses/cs468-05-fall/Papers/param-survey.pdf

Map points bijectively on surface to another domain (2D, sphere)

Surfaces that are [[Homeomorphic to a disk|geometry.mesh-parameterization#homeomorphic-to-a-disk]] are mapped to 2D plane

Application
- Texture mapping
- Surface approximation and remeshing

## Concepts
### Homeomorphic to a disk
Topologically same as a disk
#### Disk
A region in plane bounded by a circle

#### Homeomorphic
Bicontinuous function (its inverse function is also continuous) maps between topological space. It's [[Isomorphism|geometry.mesh-parameterization#isomorphism]] in topology, a.k.a. preserve same topological properities

#### Isomorphism
Structure-preserving bijective mapping

### Cauchy-Riemann equations
For complex number $z = x + iy$ and $w = u + iv$, $w = f(z)$ a.k.a.
$u(x, y) + iv(x, y) = f(x + iy)$, 

$$
\frac{\partial u}{\partial x} = \frac{\partial v}{\partial y}
$$

$$
\frac{\partial u}{\partial y} = -\frac{\partial v}{\partial x}
$$


### Developable surface
Smooth surface with zero [[Gaussian curvature|geometry.discrete-laplace-operator#gaussian-curvature]], a.k.a. one of the [[Principle curvatures|geometry.discrete-laplace-operator#principle-curvatures]] is 0

## Mapping
Suppose a suface $S \in \mathbb{R}^3$ parameterized as

$$
\bm{x}(u, v) = (x_1(u, v), x_2(u, v), x_3(u, v))
$$

where $(u, v) \in \mathbb{R}^2$

> Parameterization almost always introduces distortion in angles or areas


### Regular surface
1. $x1, x2, x3$ are differentiable
2. $\frac{\partial \bm{x}}{\partial u}$ and $\frac{\partial \bm{x}}{\partial v}$ are linear dependent (nonzero cross product)

### First fundamental form
The [[linear-algebra.inner-product]] of tangent vectors of [[Regular surface|geometry.mesh-parameterization#regular-surface]]

$$
\bm{I} = 
\begin{bmatrix} 
g_{11} & g_{12} \\
g_{12} & g_{22}
\end{bmatrix}
=
\begin{bmatrix} 
\frac{\partial \bm{x}}{\partial u}^T \cdot \frac{\partial \bm{x}}{\partial u} & \frac{\partial \bm{x}}{\partial u}^T \cdot \frac{\partial \bm{x}}{\partial v} \\
\frac{\partial \bm{x}}{\partial v}^T \cdot \frac{\partial \bm{x}}{\partial u} & \frac{\partial \bm{x}}{\partial v}^T \cdot \frac{\partial \bm{x}}{\partial v}
\end{bmatrix}
$$

$\bm{I}$ is positive definite, a.k.a. has strictly positive determinant $g_{11} g_{22} - g_{12}^2 > 0$

In [[Riemannian Metric|geometry.laplacian#riemannian-metric]] form

$$
\begin{aligned}
ds^2 &= 
\frac{\partial \bm{x}}{\partial u}^T \cdot \frac{\partial \bm{x}}{\partial u} (du)^2 + 2\frac{\partial \bm{x}}{\partial u}^T \cdot \frac{\partial \bm{x}}{\partial v} (du dv) + \frac{\partial \bm{x}}{\partial v}^T \cdot \frac{\partial \bm{x}}{\partial v} (dv)^2 \\
&=
\begin{bmatrix} du & dv \end{bmatrix} \bm{I} \begin{bmatrix} du \\ dv \end{bmatrix}
\end{aligned}
$$

### Conformal Mapping
Angle-preserving for corresponding angle between intersection arcs

### Equiareal Mapping
Area-preserving

### Isometric Mapping
Length-preserving, Conformal + Equiareal

## Surface parameterization methods
[[Isometric Mapping|geometry.mesh-parameterization#isometric-mapping]] is idea, but only hold for [[Developable surface|geometry.mesh-parameterization#developable-surface]]. The goal is usually minimizing angle distortion or area distortion or both.

### Planar mapping
$f: \mathbb{R}^2 \to \mathbb{R}^2$ $f(x, y) = (u(x, y), v(x, y))$

$$
\bm{I} = \bm{J}^T \bm{J}
$$

The eigenvalues $\lambda_1$ $\lambda_2$ of $\bm{I}$ satisfied
$$
\begin{cases}
f\text{ is isometric} & \bm{I} = \begin{bmatrix}1 & 0 \\ 0 & 1 \end{bmatrix} & \lambda_1 = \lambda_2 = 1 \\
f\text{ is conformal} & \bm{I} = \begin{bmatrix}n & 0 \\ 0 & n \end{bmatrix} & \lambda_1 / \lambda_2 = 1 \\
f\text{ is equiareal} & \det\bm{I} = 1 & \lambda_1\lambda_2 = 1 \\
\end{cases}
$$

### Conformal mapping
[[Conformal Mapping|geometry.mesh-parameterization#conformal-mapping]] satisfied [[Cauchy-Riemann equations|geometry.mesh-parameterization#cauchy-riemann-equations]]. By taking derivative w.r.t. to $x$ and $y$, we have

$$
\frac{\partial^2 u}{\partial x^2} = \frac{\partial^2 v}{\partial y \partial x}
, \
\frac{\partial^2 u}{\partial y^2} = -\frac{\partial^2 v}{\partial x \partial y}
, \
\frac{\partial^2 u}{\partial x \partial y} = \frac{\partial^2 v}{\partial y^2}
, \
\frac{\partial^2 u}{\partial y \partial x} = -\frac{\partial^2 v}{\partial x^2}
$$
then we have
$$
\frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} = 0 , \
\frac{\partial^2 v}{\partial x^2} + \frac{\partial^2 v}{\partial y^2} = 0
$$
which are two [[Laplace equations|geometry.laplacian#laplace-equation]]
$$
\Delta u = 0, \ \Delta v = 0
$$

It means [[Conformal Mapping|geometry.mesh-parameterization#conformal-mapping]] is also harmonic

#### Harmonic benefits
- Solution to linear Elliptic partial differential equation (PDE)
- Guaranteed one-to-one for convex region

## Uniform / Cotangent Laplacian mapping
See: [[Data fitting (interpolating)|geometry.laplacian#data-fitting-interpolating]]

> Local transform applied to one-ring neighbourhood

1. Find boundary vertices, map it to unit circle uv
2. Compute Uniform / Cotanagent Laplacian weight matrix $\bm{L}$
3. For each boundary row of $\bm{L}$, set it boundary index value to $1$ and others to 0 (Laplacian does not hold for boundary)
3. Build target vector $\bm{b}$, with boundary row equals to computed uv (boundary condition), non-boundary rows equals to $\bm{0}$ (Laplacian)
4. Solve $\bm{L} \bm{X} = \bm{b}$


## Least square conformal mapping (LSCM)
> Reference: https://members.loria.fr/Bruno.Levy/papers/LSCM_SIGGRAPH_2002.pdf

Define criterion $C$ that minimize the violation of [[Cauchy-Riemann equations|geometry.mesh-parameterization#cauchy-riemann-equations]]

$$
C(T) = \int_T |\frac{\partial \mathcal{U}}{\partial x} + i \frac{\partial \mathcal{U}}{\partial y}|^2 dA = |\frac{\partial \mathcal{U}}{\partial x} + i \frac{\partial \mathcal{U}}{\partial y}|^2 A_T
$$
where $\mathcal{U} = u + iv$, $T$ is a triangle, $A_T$ is the area of the triangle, $|z|$ is the modulus of complex number $z$ ($\sqrt{x^2 + y^2}$ for $z=x+iy$)

The criterion summed over whole triangulation
$$
C(\mathcal{T}) = \sum_{T \in \mathcal{T}} C(T)
$$
where $\mathcal{T}$ the set of all triangles

### Jacobian view
> Reference: http://crl.ethz.ch/teaching/shape-modeling-18/lectures/05_Mappings.pdf

We want the jacobian 
$$
\bm{J} =
\begin{bmatrix}
\frac{\partial u}{\partial x} & \frac{\partial u}{\partial y} \\
\frac{\partial v}{\partial x} & \frac{\partial v}{\partial y}
\end{bmatrix}
$$
to be a similarity matrix
$$
\begin{bmatrix}
\alpha & -\beta \\
\beta & \alpha
\end{bmatrix}
$$

Thus we have
$$
\frac{\partial u}{\partial x} = \frac{\partial v}{\partial y} \\
\frac{\partial u}{\partial y} = -\frac{\partial v}{\partial x}
$$
which is aligned with [[Cauchy-Riemann equations|geometry.mesh-parameterization#cauchy-riemann-equations]]

### Side notes

Texture atlas generation steps
- Segementation: Partition model into a set of charts, with boundary that minimize texture artifacts
- Parameterization: Unfold charts into $\mathbb{R}^2$, such that the sampling is as uniform as possible
- Packing: Gather charts optimally in texture space

Minimize angle deformation and non-uniform scaling

## TODO
- [x] Homeomorphic to a disk
- [x] Cauchy-Riemann equations
- [x] Angle deformations
- [x] Conformal Map
- [x] Isotropic
- [ ] Matrix form of LSCM solution
