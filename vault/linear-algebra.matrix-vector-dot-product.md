---
id: s353fwf4k5f883r590cj6bf
title: Matrix Vector Dot Product
desc: ''
updated: 1652418358834
created: 1648697579876
---

For a vector $\bm{v} = \begin{bmatrix} m \\ n \end{bmatrix} = m \bm{i} + n \bm{j}$

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

## Change of basis

The transformation $\bm{A}$ of $\bm{v}$ can be viewed as transformation of basis from $<\bm{i}, \bm{j}>$ to $<\begin{bmatrix} a \\ c \end{bmatrix}, \begin{bmatrix} b \\ d \end{bmatrix}>$ with linear conbination $(m, n)$ perserved. 

## Change of coordinate system

> **IMPORTANT** The result of $\bm{A} \bm{v}$ can also be viewed as how the $\bm{v}' = \begin{bmatrix} m \\ n \end{bmatrix}$ in the coordinate system $<\begin{bmatrix} a \\ c \end{bmatrix}, \begin{bmatrix} b \\ d \end{bmatrix}>$ seen in coordinate system $<\bm{i}, \bm{j}>$.

In general, suppose matrix $\bm{A} \in C$ was formed by the basis of $C'$ in $C$ coordinate system, assume $\bm{v} \in C$ and $\bm{v}' \in C'$ represent the same vector in different coordinate system

$$
\bm{A} \bm{v}' = \bm{v}
$$

$\bm{A}$ transfrom $\bm{v}'$ to the correct interpretation in $C$

$$
\bm{A}^{-1} \bm{v} = \bm{v}'
$$

$\bm{A}^{-1}$ transfrom $\bm{v}$ to the correct interpretation in $C'$

## Symmetric matrix
If $A$ is symmetric matrix
$$
(\bm{A} \cdot \bm{b})^T = \bm{b}^T \bm{A}^T = \bm{b}^T \bm{A}
$$