---
id: q4t6u51esx6mnb53i1x2nux
title: Eigendecomposition
desc: ''
updated: 1648041173388
created: 1647397739652
---

> Reference: https://mml-book.github.io/book/mml-book.pdf

## General
For square matrix $\bm{A} \in \mathbb{R}^{n \times n}$ with $n$ linear independent eigenvectors $\bm{q}_i$ and n eigenvalues $\lambda_i$

$$
\bm{A} = \bm{Q} \bm{\Lambda} \bm{Q}^{-1}
$$

where $\bm{Q} = \begin{bmatrix} 
\bm{q}_1 & \bm{q}_2 & \dots & \bm{q}_n 
\end{bmatrix}$, $\bm{\Lambda} = diag \{ \lambda_1, \lambda_2,\dots, \lambda_n \}$, $tr(\bm{\Lambda}) = \sum_{i=1}^{n} \lambda_i$

## Symmetry

> A.k.a eigenanalysis

If $\bm{A}$ is symmetric, $\bm{q}_i$ are orthonormal, $\bm{Q}$ is orthogonal ($\bm{Q}^{-1} = \bm{Q}^{T}$, $\bm{Q}\bm{Q}^{T} = \bm{Q} \bm{Q}^{T} = \bm{I}$)
$$
\bm{A} = \bm{Q} \bm{\Lambda} \bm{Q}^{T} = \sum_{i=1}^{n} \lambda_i \bm{q}_i \bm{q}_i^T
$$

If  $\bm{A}$ is singular, $rank(\bm{A}) = r < n$

$$
\bm{A} = \bm{Q}_r \bm{\Lambda}_r \bm{Q}_r^{T}
$$

where $$\bm{Q}_r \in \mathbb{R} ^ {n \times r}$$ is the matrix of the r eigenvectors with nonzero eigenvalues, $\bm{\Lambda}_r$ is diagonal matrix with corresponding nonzero eigenvalues. Note $$\bm{Q}_r^T \bm{Q}_r = \bm{I} \in \mathbb{R}^{r \times r}$$, $\bm{Q}_r \bm{Q}_r^T \neq \bm{I} \in \mathbb{R}^{n \times n}$, hence $$\bm{Q}_r$$ is no longer orthogonal