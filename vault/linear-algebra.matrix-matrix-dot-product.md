---
id: 3ag9twbpr96u06qe07fj1kt
title: Matrix Matrix Dot Product
desc: ''
updated: 1648697676581
created: 1648697676581
---

## Change of basis

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

> **IMPORTANT** The coordinates of new basis defined by $\bm{AB}$ is still in original coordinate system

## Change of coordinate system

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
