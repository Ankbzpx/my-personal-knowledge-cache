---
id: y6ura4r15hz5wtojdxfttmx
title: Basis
desc: ''
updated: 1647504321376
created: 1647342866314
---

> Reference: https://www.youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab

## Basis vectors

Linear independent vectors that span the whole vector space. All other vectors in same space can be interpreted as linear combinations of basis vectors

## Matrix
### Square Matrix

#### Determinant
Measure how area changes due to transformation, 0 means squished into lower-dimension, no inversion

#### Rank
Number of dimension in outputs (column space)

- Full rank: dimension of column space equals number of columns

-  Null space (kernel); space of all vectors land on zero vector (origin)

### Non-square Matrix
Transformation between dimensions


## Change of basis

For a vector $\bm{v} = \begin{bmatrix} m \\ n \end{bmatrix} = m \bm{i} + n \bm{j}$

Matrix vector dot product
$$
\bm{A} =
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
$$

$$
\bm{A} \bm{v} = 
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
\begin{bmatrix}
m \\ 
n 
\end{bmatrix}
=
m
\begin{bmatrix}
a \\ 
c 
\end{bmatrix}
+
n
\begin{bmatrix}
b \\ 
d 
\end{bmatrix}
$$

It can be viewed as transformation of basis from $<\bm{i}, \bm{j}>$ to $<\begin{bmatrix} a \\ c \end{bmatrix}, \begin{bmatrix} b \\ d \end{bmatrix}>$. The linear conbination perserved but basis changed.

Matrix matrix dot product

$$
\bm{B} =
\begin{bmatrix}
e & f \\
g & h
\end{bmatrix}
$$

$\bm{A} \bm{B}$ can be viewed as transform of basis of $\bm{B}$ into new basis of $\bm{A} \bm{B}$

$$
\bm{A} \bm{B} =
\begin{bmatrix}

e
\begin{bmatrix}
a \\ 
c 
\end{bmatrix}
+
g
\begin{bmatrix}
b \\ 
d 
\end{bmatrix}

&
f
\begin{bmatrix}
a \\ 
c 
\end{bmatrix}
+
h
\begin{bmatrix}
b \\ 
d 
\end{bmatrix}

\end{bmatrix}
$$

### Matrix vector dot product

Suppose matrix $\bm{A} \in C$ was formed by the basis of $C'$ in $C$ coordinate system, then A transform the coordinate system from $C$ to $C'$

Assume $\bm{v} \in C$ and $\bm{v}' \in C'$ represent the same vector

$$
\bm{A} \bm{v}' = \bm{v}
$$

$\bm{A}$ transfrom $\bm{v}'$ to the correct interpretation in $C$

$$
\bm{A}^{-1} \bm{v} = \bm{v}'
$$

$\bm{A}^{-1}$ transfrom $\bm{v}$ to the correct interpretation in $C'$

### Matrix matrix dot product

$$
\bm{B} = \bm{A}^{-1} \bm{M} \bm{A} \\

\begin{aligned}
\\
&\bm{A}: \text{transformation of basis matrix } C \rarr C'\\
&\bm{M}: \text{transformation } \in C \\
&\bm{A}^{-1}: \text{inverse transformation of basis matrix } C' \rarr C \\
&\bm{B}: \text{equivalent transformation of } \bm{M} \in C'
\end{aligned}
$$

$\bm{M}$ is transformation you see it, $\bm{A}$ represents shift in perspective, $\bm{A}^{-1} \bm{M} \bm{A}$ represents same transformation someelse sees it

## Dot product

Given 
$$
\bm{a} = a_1 \bm{i} + a_2 \bm{j} + a_3 \bm{k} \\
\bm{b} = b_1 \bm{i} + b_2 \bm{j} + b_3 \bm{k}
$$
where ($\bm{i}$, $\bm{j}$, $\bm{k}$) are orthonormal basis


### Algebra
$$
\bm{a} \cdot \bm{b} = 

\begin{bmatrix}
a_1 & a_2 & a_3
\end{bmatrix}

\begin{bmatrix}
b_1 \\ b_2 \\ b_3
\end{bmatrix}


= a_1 b_1 + a_2 b_2 + a_3 b_3
$$

Row vector ($1 \times n$ matrix) as linear transformation of vector to number, it projects each basis vector to number line

$$
\bm{i} \rarr a_1 \\
\bm{j} \rarr a_2 \\
\bm{k} \rarr a_3
$$


### Geometry
$$
\begin{aligned}

\bm{a} \cdot \bm{b} &= \| \text{projection of } \bm{a}\text{ on }\bm{b} \| \cdot \|\bm{b}\| = \| \bm{a} \| \cos{\theta} \| \bm{b} \| \\
&= \| \text{projection of } \bm{b}\text{ on }\bm{a} \| \cdot \|\bm{a}\| = \| \bm{b} \| \cos{\theta} \| \bm{a} \| \\

\end{aligned}
$$


## Cross product

Given 
$$
\bm{a} = a_1 \bm{i} + a_2 \bm{j} + a_3 \bm{k} \\
\bm{b} = b_1 \bm{i} + b_2 \bm{j} + b_3 \bm{k}
$$
where ($\bm{i}$, $\bm{j}$, $\bm{k}$) are orthonormal basis


### Vector-Vector

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

### Geometry
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

### Matrix-Vector

#### skew-symmetric matrix

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


### Properties

1. Magnitude of cross product $\|\bm{a} \times \bm{b}\|$ defines the positive area of parallelogram with $\bm{a}$ and $\bm{b}$ as sides. Equivalent of the determinant of the matrix form

2. Cross product of 2 linear dependent vectors
$\bm{a}$ and $\bm{b}$ is zero vector (area is 0)
$$
\bm{a} \times \bm{b} = \bm{0}
$$

## Eigenvector & Eigenvalues
For non-zero $\bm{v}$ that satisfy
$$
\bm{A} \bm{v} = \lambda \bm{v} = \lambda \bm{I} \bm{v} \\
$$
So
$$
(\bm{A} - \lambda \bm{I}) \bm{v} = \bm{0}
\\
\det{(\bm{A} - \lambda \bm{I})} = 0
$$

$\bm{v}$ is a eigenvector of $\bm{A}$ with corresponding eigenvalue $\lambda$. Note eigenvectors only scales during transformation


### Eigenvalue properties

**Complex eigenvalues**: correspond to rotation in transformation, no eigenvectors

**Eigenvalue euqal to 1**: fix in place during transformation

### Eigenbasis
Choose a set of eigenvectors that span whole space, use eigenvectors as basis vector

$$
\bm{Q}^{-1} \bm{A} \bm{Q} = \bm{\Lambda} \\
\bm{A} = \bm{Q} \bm{\Lambda} \bm{Q}^{-1}  
$$

See: [[Eigen decomposition|linear-algebra.decomposition#eigen-decomposition]]

Transformation applied on eigen basis is guaranteed to be diagonal

To compute chain multiplication of same matrix n times, its easier to change it to eigenbasis, transform n times then change it back. See [[Change of basis|linear-algebra.basis#change-of-basis]]

$$
\bm{A}^n = \bm{Q} \bm{\Lambda}^n \bm{Q}^{-1}
$$

### Relation to rank

> Reference: https://math.stackexchange.com/questions/1349907/what-is-the-relation-between-rank-of-a-matrix-its-eigenvalues-and-eigenvectors

For square matrix $\bm{A} \in \mathbb{R}^{n \times n}$, $n = \text{rank} + \text{nullity}$, kernel of $\bm{A}$ is the eigenspace with 0 eigenvalue
