---
id: mzeqbcey7i1rxwuh0hdyy2g
title: Laplacian
desc: ''
updated: 1649123599965
created: 1649123599965
---

## Concepts

### Circumcenter
> Reference: https://mathworld.wolfram.com/Circumcenter.html

For all edges, the line segement from circumcenter to edge midpoint is perpendicular to that edge. 

It is the center of the triangle's circumcircle, with its radius equal to distance between circumcenter to all vertices.

The coordinate of circumcenter w.r.t. vertex angles (Trilinear Coordinates) can be written as:
$$
R \cos A : R \cos B: R \cos C
$$

### Law of cosines
$$
\cos A = \frac{b^2 + c^2 - a^2}{2bc}
$$

###  Law of sines
$$
\frac{\sin A}{a} = \frac{\sin B}{b}
$$

### Cotangent

$$
\cot \theta = \frac{1}{\tan \theta} = \frac{\cos \theta}{\sin \theta}
$$

#### Formula
For a triangle of vertices $\{A, B, C\}$ with opposite edges $\{a, b, c\}$

Suppose we know the aera of the triangle $Area$, the height $h$ of base $a$ is 

$$
h = \frac{Aera}{a} \\
$$
We can express $\sin B$ with $h$ as
$$
\sin B = \frac{h}{c} = \frac{Aera}{ac}
$$

Then with [[Law of sines|code-read.igl.laplacian#law-of-sines]]

$$
\sin A = \frac{a \sin B}{b} = \frac{Aera}{bc}
$$

Thus, combined with [[Law of cosines|code-read.igl.laplacian#law-of-cosines]]:

$$
\cot A = \frac{\cos A}{\sin A} = \frac{b^2 + c^2 - a^2}{0.5 * Aera}
$$

## Implementation
### cotmatrix
1. For each face, for edges {(1, 2), (2, 0), (0, 1)} in face, compute [[cotmatrix_entries|code-read.igl.laplacian#cotmatrix_entries]] for triplets
2. Form SparseMatrix by `setFromTriplets`. Note both orientations of same edge are appended, which will be [summed up](https://eigen.tuxfamily.org/dox/classEigen_1_1SparseMatrix.html#a8f09e3597f37aa8861599260af6a53e0) during sparse construction.

> IMPORTANT!

$$
L_{ij} = 
\begin{cases}
\cot \alpha_{ij} + \cot \beta_{ij} & j\in N_1(i) \\
0 & j\notin N_1(i) \\
-\sum_{k \neq i} L_ik & i=j
\end{cases}
$$

Suppose row $i$ of $L \in \mathbb{R}^{NV \times NV}$ is
$$
\begin{bmatrix}
L_a & 0 & \dots & 0 & L_b & 0 & \dots & 0 &  -(L_a + L_b + L_c + L_d) & 0 & \dots & 0 &  L_c & 0 & \dots & 0 &  L_d
\end{bmatrix}
$$
$V \in \mathbb{R}^{NV \times 3}$ is the matrix of vertices

Then the row $i$ of $-L \cdot V$ is

$$
\begin{aligned}
&\begin{bmatrix}
(L_a + L_b + L_c + L_d) \cdot x_i - (L_a x_a + L_b x_b + L_c x_c + L_d x_d) \\
(L_a + L_b + L_c + L_d) \cdot y_i - (L_a y_a + L_b y_b + L_c y_c + L_d y_d) \\
(L_a + L_b + L_c + L_d) \cdot z_i - (L_a z_a + L_b z_b + L_c z_c + L_d z_d)
\end{bmatrix}^T \\
=
&\begin{bmatrix}
L_a(x_i - x_a) + L_b(x_i - x_b) + L_c(x_i - x_c) + L_d(x_i - x_d) \\
L_a(y_i - y_a) + L_b(y_i - y_b) + L_c(y_i - y_c) + L_d(y_i - y_d) \\
L_a(z_i - z_a) + L_b(z_i - z_b) + L_c(z_i - z_c) + L_d(z_i - z_d)
\end{bmatrix}^T \\
= &\sum_{j \in \{a, b, c, d\}} L_{ij} (\bm{x}_i - \bm{x}_j)
\end{aligned}
$$

which is equivalent to the definition in [[Discrete Mean curvature Normal|geometry.discrete-laplace-operator#discrete-mean-curvature-normal]]

### cotmatrix_entries
1. Compute [[squared_edge_lengths|code-read.igl.laplacian#squared_edge_lengths]]
2. Compute [[doublearea|code-read.igl.laplacian#doublearea]] with edge_length
3. For `squared_edge_lengths` of each face {a^2, b^2, c^2}, compute cotangent with [[Formula|code-read.igl.laplacian#formula]] as `(a^2 + b^2 - c^2) / doublearea / 4.0`

### squared_edge_lengths
For each triangle face, compute `squaredNorm` for `edge` in {(1, 2), (2, 0), (0, 1)}, shape of NF x 3

### doublearea
Input: NF x 3 edge length

Compute double triangle area using **Kahan's Heron's formula** to avoid numerical issues
1. Sort edge length `a`, `b`, `c`
2. Compute `arg = (a + (b + c)) * (c - (a - b)) * (c + (a - b)) + (a + (b - c))`
3. `doublearea = 2.0 * 0.25 * sqrt(arg)`
4. Perform [[coding.cpp.nan-check]] and replace `NaN` with pre-specified replacement value


### massmatrix
1. Compute `edge_length`, NF x 3
2. Call [[massmatrix_intrinsic|code-read.igl.laplacian#massmatrix_intrinsic]]

### massmatrix_intrinsic
Compute [[doublearea|code-read.igl.laplacian#doublearea]] $dblA \in \mathbb{R}^{NF \times 3}$
#### MASSMATRIX_TYPE_BARYCENTRIC
For $F = 
\begin{bmatrix}
c_0 & c_1 & c_2
\end{bmatrix} \in \mathbb{R}^{NF \times 3}$, build SparseMatrix by $(x, y)$ indices of 
$$
\begin{bmatrix}
c_0 \\
c_1 \\
c_2
\end{bmatrix} \in \mathbb{R}^{3NF}
$$
and value of (using [[repmat|code-read.igl.laplacian#repmat]])
$$
\begin{bmatrix}
dblA/6 \\
dblA/6 \\
dblA/6
\end{bmatrix} \in \mathbb{R}^{3NF \times 3}
$$
A.k.a. each oriented edge (opposite vertex) has mass $dblA/6$.

Note that the resulting SparseMatrix is a diagonal matrix with shape NV x NV

#### MASSMATRIX_TYPE_VORONOI
Same indices as above, but with voronoi triangle area computed as:
1. Compute cosines using [[Law of cosines|code-read.igl.laplacian#law-of-cosines]]
2. Divide triangle area for vertex by its cosine times opposite edge_length (length of projection of circumcenter to each edge is porportional to the cosine of opposite vertex angle)
3. Handle [[Obtuse triangle|geometry.discrete-laplace-operator#obtuse-triangle]] as described in [[Discrete Mean curvature Normal|geometry.discrete-laplace-operator#discrete-mean-curvature-normal]]

### repmat
Like `np.repeat`, supporting SparseMatrix

## TODO
- [ ] [Kahan's Heron's formula](https://people.eecs.berkeley.edu/~wkahan/Triangle.pdf)
- [ ] Triangle edge length inequality
- [x] Cotangent formula