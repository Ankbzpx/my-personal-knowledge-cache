
Given 
$$
\bm{a} = a_1 \bm{i} + a_2 \bm{j} + a_3 \bm{k} \\
\bm{b} = b_1 \bm{i} + b_2 \bm{j} + b_3 \bm{k}
$$
where ($\bm{i}$, $\bm{j}$, $\bm{k}$) are orthonormal basis


## Vector-Vector

$$
\begin{aligned}

\bm{a} \times \bm{b} &= 
\begin{vmatrix}
\bm{i} & \bm{j} & \bm{k} \\
a_1 & a_2 & a_3 \\
b_1 & b_2 & b_3
\end{vmatrix} \\

&=

\begin{vmatrix}
a_2 & a_3 \\
b_2 & b_3
\end{vmatrix}

\bm{i} -

\begin{vmatrix}
a_1 & a_3 \\
b_1 & b_3
\end{vmatrix}

\bm{j} +

\begin{vmatrix}
a_1 & a_2 \\
b_1 & b_2
\end{vmatrix}

\bm{k} \\

&= (a_2 b_3 - a_3b_2) \bm{i} - (a_1 b_3 - a_3 b_1) \bm{j} + (a_1 b_2 - a_2 b_1) \bm{k}

\end{aligned}

$$

## Geometry view
$$
\bm{p} \cdot \bm{x} = 
\det{(
\begin{bmatrix}
\bm{x} & \bm{a} & \bm{b}
\end{bmatrix})}
$$

for any vector $\bm{x}$

Assume:
- $\bm{c}$ is the vector that perpendicular to $\bm{a}$ and $\bm{b}$
- $a$ is the area of parallelogrom spanned out by $\bm{a}$ and $\bm{b}$
- $V$ is the volume of space spanned out by $\bm{x}$, $\bm{a}$ and $\bm{b}$

We have
$$
V = \| \text{projection of } \bm{x} \text{ on } \bm{c} \| \cdot a =
\bm{x} \cdot (a \cdot \bm{c})
$$

So $\bm{p} = a \cdot \bm{c}$, hence the cross product

## Matrix-Vector

### skew-symmetric matrix

$$
\bm{A}^{T} = - \bm{A}
$$

Define skew-symmetric matrix $[\bm{a}]_{\times}$ as

$$
[\bm{a}]_{\times} = 
\begin{bmatrix}
0 & -a_3 & a_2 \\
a_3 & 0 & -a_1 \\
-a_2 & a_1 & 0
\end{bmatrix}
$$

$$
\begin{aligned}

\bm{a} \times \bm{b} = [\bm{a}]_{\times} \bm{b} &= 

\begin{bmatrix}
0 & -a_3 & a_2 \\
a_3 & 0 & -a_1 \\
-a_2 & a_1 & 0
\end{bmatrix}

\begin{bmatrix}
b_1 \\
b_2 \\
b_3
\end{bmatrix} \\

&= \begin{bmatrix}
0 & -a_3 b_2 & a_2 b_3 \\
a_3 b_1 & 0 & -a_1 b_3 \\
-a_2 b_1 & a_1 b_2 & 0
\end{bmatrix}

\end{aligned}
$$


## Properties

1. Magnitude of cross product $\|\bm{a} \times \bm{b}\|$ defines the positive area of parallelogram with $\bm{a}$ and $\bm{b}$ as sides. Equivalent of the determinant of the matrix form

2. Cross product of 2 linear dependent vectors
$\bm{a}$ and $\bm{b}$ is zero vector (area is 0)
$$
\bm{a} \times \bm{b} = \bm{0}
$$
