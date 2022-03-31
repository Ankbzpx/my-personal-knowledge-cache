---
id: 6cxfvsckp494oiut6rwbgqd
title: Dot Product
desc: ''
updated: 1648696241608
created: 1648696241608
---
Given 
$$
\bm{a} = a_1 \bm{i} + a_2 \bm{j} + a_3 \bm{k} \\
\bm{b} = b_1 \bm{i} + b_2 \bm{j} + b_3 \bm{k}
$$
where ($\bm{i}$, $\bm{j}$, $\bm{k}$) are orthonormal basis


## Algebra View
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


## Geometry view

$$
\begin{aligned}

\bm{a} \cdot \bm{b} &= \| \text{projection of } \bm{a}\text{ on }\bm{b} \| \cdot \|\bm{b}\| = \| \bm{a} \| \cos{\theta} \| \bm{b} \| \\
&= \| \text{projection of } \bm{b}\text{ on }\bm{a} \| \cdot \|\bm{a}\| = \| \bm{b} \| \cos{\theta} \| \bm{a} \| \\

\end{aligned}
$$
