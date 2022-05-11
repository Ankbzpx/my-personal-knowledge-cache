---
id: qc5d3qlm62yv4xedppp0ze1
title: Point Line and Plane
desc: ''
updated: 1652238438499
created: 1652148686489
---

> Reference: https://www.robots.ox.ac.uk/~vgg/hzbook/

## 2D (planar)

### Point
#### Euclidean
$\bm{x} = (x, y)^T \in \mathbb{R}^2$

#### Homogenous 
Normalized $\bm{x} = (x, y, 1)^T$, unnormalized $\bm{x} = (kx, ky, k)^T$

### Line
$\bm{l} = (a, b, c)^T$, unnormalized $\bm{l} = (ka, kb, kc)^T$

> Can be written as $y = -\frac{a}{b} x -\frac{c}{b}$. determined by gradient $-\frac{a}{b}$ and $y$ interception $-\frac{c}{b}$, 2 DoF

### Point on line
For point $\bm{x} = (x, y)^T$ on line $\bm{l} = (a, b, c)^T$, $ax + by + c = 0$

vector form with [[linear-algebra.dot-product]]
$$
\bm{x}^T \bm{l} = \begin{bmatrix} x & y & 1 \end{bmatrix} \begin{bmatrix} a \\ b \\ c \end{bmatrix} = 0
$$

### Intersection of lines
For lines $\bm{l} = (a, b, c)^T$, $\bm{l'} = (a', b', c')^T$

Define $\bm{x} = \bm{l} \times \bm{l'}$ with [[linear-algebra.cross-product]]

$$
\bm{l} \cdot (\bm{l} \times \bm{l'}) = \bm{l'} \cdot (\bm{l} \times \bm{l'}) = 0 \\
\bm{l} \cdot \bm{x} = \bm{l'} \cdot \bm{x} = 0
$$
Thus, $\bm{x}$ is the intersection of $\bm{l}$ and $\bm{l'}$

### Line passing through points
For points $\bm{x} = (x, y, z)^T$, $\bm{x'} = (x', y', z')^T$

Define $\bm{l} = \bm{x} \times \bm{x'}$ with [[linear-algebra.cross-product]]

$$
\bm{x} \cdot (\bm{x} \times \bm{x'}) = \bm{x'} \cdot (\bm{x} \times \bm{x'}) = 0 \\
\bm{x} \cdot \bm{l} = \bm{x'} \cdot \bm{l} = 0
$$
Thus, $\bm{l}$ is the line that passing through $\bm{x}$ and $\bm{x'}$

### Intersection of parallel lines (idea points)
For parallel lines $\bm{l} = (a, b, c)^T$, $\bm{l'} = (a, b, c')^T$

Their [[Intersection|epipolar-geometry.point-line-and-plane#intersection-of-lines]] can be computed as
$$

\begin{aligned}

\bm{l} \times \bm{l'} &= 
\begin{vmatrix}
i & j & k \\
a & b & c \\
a & b & c'
\end{vmatrix}
\\&=
\begin{vmatrix}
b & c \\
b & c'
\end{vmatrix} i
-
\begin{vmatrix}
a & c \\
a & c'
\end{vmatrix} j
+
\begin{vmatrix}
a & b \\
a & b
\end{vmatrix} k
\\&=
b(c'-c)i - a(c'-c)j + 0k
\\&=
(c'-c)(b, a, 0)^T
\end{aligned}
$$

$\bm{x} = (b, a, 0)^T$ is the [[Point at infinity (Idea points)|epipolar-geometry.euclidean-and-projective#points-at-infinity-idea-points]], $(c'-c)$ is the scaling factor

### Line passing through idea points (line at infinity)
For idea points $\bm{x} = (x, y, 0)^T$, $\bm{x'} = (x', y', 0)^T$

[[Line passing through points|epipolar-geometry.point-line-and-plane#line-passing-through-points]] can be computed as

$$
\begin{aligned}
\bm{x} \times \bm{x'}& = 
\begin{vmatrix}
i & j & k \\
x & y & 0 \\
x' & y' & 0
\end{vmatrix}
\\&=
\begin{vmatrix}
y & 0 \\
y & 0
\end{vmatrix} i
-
\begin{vmatrix}
x & 0 \\
x & 0
\end{vmatrix} j
+
\begin{vmatrix}
x & y \\
x' & y'
\end{vmatrix} k
\\& = 0i + 0j + (xy' - x'y)k
\\& = (xy' - x'y) (0, 0, 1)^T
\end{aligned}
$$

$\bm{l} = (0, 0, 1)^T$ is the [[Line at infinity|epipolar-geometry.euclidean-and-projective#line-at-infinity]], $(xy' - x'y)$ is the scaling factor

## 3D
### Point
#### Euclidean
$\bm{x} = (x, y, z)^T \in \mathbb{R}^3$

#### Homogenous
$\bm{x} = (x, y, z, 1)^T$

### Plane
$\bm{\pi} = (\pi_1, \pi_2, \pi_3, \pi_4)^T$

> Similar to line in planar case, determined by gradient $(-\frac{\pi_1}{\pi_3}, -\frac{\pi_2}{\pi_3})$ and intersection with $z$ axis $-\frac{\pi_4}{\pi_3}$, 3 DoF

### Point on plane
$$
\pi_1 x + \pi_2 y + \pi_3 z + \pi_4 = 0
$$
#### Euclidean
$$
\bm{n}^T \bm{x} + d = 0
$$
where $\bm{n} = (\pi_1, \pi_2, \pi_3)^T$, $\frac{\bm{n}}{\|\bm{n}\|}$ is plane normal, $d = \pi_4$, $d/\|\bm{n}\|$ is the distance of plane from origin

##### Distance from a point $\bm{p}$ to plane $(\bm{n}, d)$

Denote $\bm{q}$ as any point on plane, distance of $\bm{p}$ on plane is the projection of segement $\bm{p} - \bm{q}$ on plane normal $\bm{n}$

$$
\|\frac{\bm{n}}{\|\bm{n}\|}^T (\bm{p} - \bm{q})\|_2^2
$$

#### Homogenous
$$
\bm{\pi}^T\bm{x} = 0
$$
where $\bm{\pi} = (\pi_1, \pi_2, \pi_3, \pi_4)^T$

### Point, Plane, Line relation
- 3 distinct points, or 1 line and 1 point (the point must not be incident with the line) define a plane
- 2 distict planes join 1 line
- 3 distict planes intersect 1 point
