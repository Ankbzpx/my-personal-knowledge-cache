---
id: ycrws7ju941szwpw73ef9m3
title: QR Decomposition
desc: ''
updated: 1648697209124
created: 1648697209124
---

> Reference: https://mml-book.github.io/book/mml-book.pdf


For any real matrix $\bm{A} \in \mathbb{R}^{n \times n}$ can be decomposed into
$$
\bm{A} = \bm{Q} \bm{R}
$$
where $\bm{Q}$ is orthogonal matrix, $\bm{R}$ is upper triangle matrix

### Linear system
$$
\bm{A} \bm{x} = \bm{b} \\
\bm{Q} \bm{R} \bm{x} = \bm{b}
$$
Such that
$$
\bm{R} \bm{x} = \bm{Q} ^ T \bm{b}
$$
which is easier to solve as $\bm{R}$ is upper triangle matrix

### Eigenvalue computation
For matrix $\bm{A}$, create series of $\bm{A}_k$, start with $k=0$, $\bm{A}_0 = \bm{A}$
$$
\bm{A}_k = \bm{Q}_k \bm{R}_k \\
\bm{A}_{k+1} = \bm{R}_k \bm{Q}_k
$$
So that
$$
\bm{A}_{k+1} = \bm{R}_k \bm{Q}_k = \bm{Q}_k^T \bm{Q}_k \bm{R}_k \bm{Q}_k = \bm{Q}_k^T \bm{A}_k \bm{Q}_k
$$
$\bm{A}_{k+1}$ converge to a triangular matrix with diagonal containing eigenvalues