---
id: l80yjov1nte0nt9ueo9tqi2
title: Cross Product
desc: ''
updated: 1646996418155
created: 1646976562517
---

## Definition

Given 
$$
\bm{a} = a_1 \bm{i} + a_2 \bm{j} + a_3 \bm{k} \\
\bm{b} = b_1 \bm{i} + b_2 \bm{j} + b_3 \bm{k}
$$
where ($\bm{i}$, $\bm{j}$, $\bm{k}$) are orthonormal basis,

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

\bm{k}

\end{aligned}

$$

## Properties

1. Magnitude of cross product $\|\bm{a} \times \bm{b}\|$ defines the positive area of parallelogram with $\bm{a}$ and $\bm{b}$ as sides.

2. Cross product of 2 linear dependent vectors
$\bm{a}$ and $\bm{b}$ is zero vector (area is 0)
$$
\bm{a} \times \bm{b} = \bm{0}
$$