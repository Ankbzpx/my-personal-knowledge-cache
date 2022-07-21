
> Reference: http://www.cs.harvard.edu/~sjg/papers/arap.pdf

## Concepts
### Frobenius matrix norm
$$
\|\bm{A}\|_F = \sqrt{tr(\bm{A}^T\bm{A})}
$$

$$
\|\bm{A}\|_F = (\sum_{i,j} |\bm{A}_{ij}|^2)^{\frac{1}{2}}
$$

## Objective function

Find optimal local transformation for each individual triangle then stitching transformed triangle to 2D mesh

Define energy function
$$
E(u, \bm{L}) = \sum_{t=1}^T A_t \|\bm{J}_t(u) - \bm{L}_t\|_F^2
$$
where $A_t$ is triangle area, $\bm{J}_t(u)$ is Jacobian of triangle, $\bm{L}_t$ is auxiliary linear transformation

$$
(u, \bm{L}) = \argmin_{(u, \bm{L})} E(u, \bm{L}) \ s.t. u, \bm{L}_t \in \bm{M}
$$
where $\bm{M}$ is the set of allowed linear transformation (similarity and rotation)

## Matrix approximation
To approximate $\bm{J} \in \mathbb{R}^{2 \times 2}$ with $\bm{L} \in \mathbb{R}^{2 \times 2}$, define distance function with [[Frobenius matrix norm|geometry.local-global-mesh-parameterization#frobenius-matrix-norm]]
$$
d(\bm{J}, \bm{L}) = \| \bm{J} - \bm{L} \|_F^2
$$

### Signed SVD solution

Recall [[linear-algebra.decomposition.singular-value-decomposition]], $\bm{J} \in \mathbb{R}^{2 \times 2}$ can be decomposed into
$$
\bm{J} = \bm{U} \bm{\Sigma} \bm{V}^T
$$
where $\bm{U}$ and $\bm{V}$ are equally sized orthogonal basis matices (a.k.a. rotation or reflection)

We constraint $\det (\bm{U} \bm{V}^T)$ to be positive by defining
$$
\Sigma =
\begin{bmatrix}
\sigma_1 & 0 \\
0 & -\sigma_2
\end{bmatrix}
$$
where $sign (\det J) = sign (\sigma_2)$

## As Similar As Possible (ASAP)
> Reference: http://crl.ethz.ch/teaching/shape-modeling-18/lectures/05_Mappings.pdf

> More see: [[Similarity|epipolar-geometry.euclidean-and-projective#similarity]]

$$
d = \| \bm{J} - \bm{L}_S \|_F^2
$$
where $\bm{L}_S = \begin{bmatrix} \alpha & -\beta \\ \beta & \alpha \end{bmatrix}$ denotes the closest similarity matrix  to Jacobian $\bm{J}$

Using [[Signed SVD solution|geometry.local-global-mesh-parameterization#signed-svd-solution]]
$$
\begin{aligned}
d = \| \bm{J} - \bm{L}_S \|_F^2 
=& \| \bm{U} \bm{\Sigma} \bm{V}^T - \bar{\sigma} \bm{U} \bm{V}^T \|_F^2 \\
=& \| \bm{U} (\bm{\Sigma} - \bar{\sigma} \bm{I}) \bm{V}^T\|_F^2 \\
=& \| \bm{\Sigma} - \bar{\sigma} \bm{I} \|_F^2 \\
=& (\sigma_1 - \bar{\sigma})^2 + (\sigma_2 - \bar{\sigma})^2
\end{aligned}
$$

The optimal $\bar{\sigma} = \frac{1}{2}(\sigma_1 + \sigma_2)$ so
$$
d = (\sigma_1 - \sigma_2)^2
$$

The trivial solution is collapses all of vertices to a single point (0 energy). It can be avoided by pinning two vertices (usually most distant away) to two pre-determined positions in plane.

### Relation to Comformal Mapping

Let
$$
\bm{J} = \begin{bmatrix} \frac{\partial u}{\partial x} & \frac{\partial u}{\partial y} \\ \frac{\partial v}{\partial x} & \frac{\partial v}{\partial y} \end{bmatrix} = \begin{bmatrix} a & b \\ c & d \end{bmatrix}
$$
which can be decomposed into
$$
\bm{J} =
\frac{1}{2} \begin{bmatrix} a + d & c - b \\ b - c & a + d \end{bmatrix} + \frac{1}{2} \begin{bmatrix} a - d & c + b \\ b + c & d - a \end{bmatrix}
$$
where $\begin{bmatrix} a + d & c - b \\ b - c & a + d \end{bmatrix}$ is the similarity part and $\begin{bmatrix} a - d & c + b \\ b + c & d - a \end{bmatrix}$ is the anti-similarity part. Thus, the goal become
$$
\begin{aligned}
d =& \| \begin{bmatrix} a - d & c + b \\ b + c & d - a \end{bmatrix} \|_F^2 \\
=& (a - d)^2 + (b + c)^2 \\
=& (\frac{\partial u}{\partial x} - \frac{\partial v}{\partial y})^2 + (\frac{\partial u}{\partial y} + \frac{\partial v}{\partial x})^2
\end{aligned}
$$

which is equivalent to [[Least square conformal mapping (LSCM)|geometry.mesh-parameterization#jacobian-view]]

## As Rigid As Possible (ARAP)
> Reference: http://crl.ethz.ch/teaching/shape-modeling-18/lectures/05_Mappings.pdf

> More see: [[Rigid|epipolar-geometry.euclidean-and-projective#rigid]]

> We only consider Rotation(angle and area preserving) here

$$
d = \| \bm{J} - \bm{L}_R \|_F^2github
$$
where $\bm{L}_R = \begin{bmatrix} \cos \theta & \sin \theta \\ -\sin \theta & \cos \theta \end{bmatrix}$ denotes the closest rotation matrix to Jacobian $\bm{J}$


Using [[Signed SVD solution|geometry.local-global-mesh-parameterization#signed-svd-solution]]
$$
\begin{aligned}
d = \| \bm{J} - \bm{L}_R \|_F^2 
=& \| \bm{U} \bm{\Sigma} \bm{V}^T - \bm{U} \bm{V}^T \|_F^2 \\
=& \| \bm{U} (\bm{\Sigma} - \bm{I}) \bm{V}^T\|_F^2 \\
=& \| \bm{\Sigma} - \bm{I} \|_F^2 \\
=& (\sigma_1 - 1)^2 + (\sigma_2 - 1)^2
\end{aligned}
$$

## TODO
- [ ] Triangle Jacobian
- [x] Jacobian as distortion
