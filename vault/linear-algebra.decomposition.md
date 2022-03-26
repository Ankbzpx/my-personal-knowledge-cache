---
id: q4t6u51esx6mnb53i1x2nux
title: Decomposition
desc: ''
updated: 1648041173388
created: 1647397739652
---

> Reference: https://mml-book.github.io/book/mml-book.pdf

## Eigen decomposition

### General
For square matrix $\bm{A} \in \mathbb{R}^{n \times n}$ with $n$ linear independent eigenvectors $\bm{q}_i$ and n eigenvalues $\lambda_i$

$$
\bm{A} = \bm{Q} \bm{\Lambda} \bm{Q}^{-1}
$$

where $\bm{Q} = \begin{bmatrix} 
\bm{q}_1 & \bm{q}_2 & \dots & \bm{q}_n 
\end{bmatrix}$, $\bm{\Lambda} = diag \{ \lambda_1, \lambda_2,\dots, \lambda_n \}$, $tr(\bm{\Lambda}) = \sum_{i=1}^{n} \lambda_i$

### Symmetry

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

## QR decomposition
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

## Singular Value Decomposition (SVD)
For matrix $\bm{A} \in \mathbb{R}^{m \times n}$
$$
\bm{A} = \bm{U} \bm{\Sigma} \bm{V}^T
$$
where $\bm{U} \in \mathbb{R}^{m \times m}$, $\bm{V} \in \mathbb{R}^{n \times n}$, $\bm{\Sigma} = diag\{\sigma_1, \sigma_2, \dots, \sigma_p\} \in \mathbb{R}^{m \times n}$, $p = \min\{m, n\}$. $\sigma_1 \geq \sigma_2 \geq \dots \geq \sigma_p \geq 0$ are singular values of $\bm{A}$

Define r as $\sigma_1 \geq \sigma_2 \geq \dots \geq \sigma_r \geq \sigma_{r+1} = \dots = \sigma_p = 0$, $rank(\bm{A}) = r$

$$
\bm{A} = \sum_{i=1}^{r} \sigma_i \bm{u}_i \bm{v}_i^T
$$

Full rank means $r = p$

### Thin SVD

If $m \geq n$
$$
\bm{A} = \bm{U}_1 \bm{\Sigma}_1 \bm{V}^T
$$
where $\bm{U}_1 \in \mathbb{R}^{m \times n}$, $\bm{\Sigma}_1 \in \mathbb{R}^{n \times n}$. Note that $$\bm{U}_1^T \bm{U}_1 = \bm{I} \in \mathbb{R}^{n \times n}$$, $\bm{U}_1 \bm{U}_1^T \neq \bm{I} \in \mathbb{R}^{m \times m}$

### Feature vector

Given $\bm{X} \in \mathbb{R}^{f \times n}$, $n \ll f$

Let $rank(\bm{X}) = k \leq \min(f, n) = n$

$$
\bm{X} \bm{X}^T = \bm{U} \bm{\Sigma} \bm{V}^T \bm{V} \bm{\Sigma} \bm{U}^T = \bm{U} \bm{\Sigma}^2 \bm{U}^T = \bm{U}_k \bm{\Sigma}_k^2 \bm{U}_k^T \in \mathbb{R}^{k \times k}
\\
\bm{X}^T \bm{X} = \bm{V} \bm{\Sigma} \bm{U}^T \bm{U} \bm{\Sigma} \bm{V}^T = \bm{V} \bm{\Sigma}^2 \bm{V}^T = \bm{V}_k \bm{\Sigma}_k^2 \bm{V}_k^T \in \mathbb{R}^{k \times k}
$$

It is equivalent to Eigen decompostion, where $\bm{\Lambda} = \bm{\Sigma}_k^2$, $\bm{U}_k \in \mathbb{R}^{f \times k}$, $\bm{V}_k \in \mathbb{R}^{n \times k}$

$$
\bm{X} \bm{X}^T = \bm{U}_k \bm{\Lambda} \bm{U}_k^T \\
\bm{X}^T \bm{X} = \bm{V}_k \bm{\Lambda} \bm{V}_k^T
$$

#### Proof $\bm{X} \bm{X}^T$ and $\bm{X}^T \bm{X}$ share the same $\bm{\Lambda}$

Let $\bm{\Lambda} = \bm{\Sigma}_k^2$

Assume $\bm{v}$ is any eigenvector of $\bm{X}^T \bm{X}$ with non-zero eigenvalue $\lambda$

$$
\bm{X}^T \bm{X} \bm{v} = \lambda \bm{v} \\
\bm{X} \bm{X}^T \bm{X} \bm{v} = \lambda \bm{X} \bm{v} \\
(\bm{X} \bm{X}^T) \bm{X} \bm{v} = \lambda (\bm{X} \bm{v}) \\
\therefore \bm{X} \bm{v} \text{ is eigenvector of }\bm{X} \bm{X}^T \text{ corresponds to same eigenvalue } \lambda \\
\therefore \bm{X}^T \bm{X}, \bm{X} \bm{X}^T \text{ have the same } \bm{\Lambda}
$$

#### Proof $\bm{U}_k = \bm{X} \bm{V}_k \bm{\Lambda} ^ {-\frac{1}{2}}$

$$
\bm{X} \bm{X}^T \bm{X} \bm{X}^T =\bm{X} (\bm{X}^T \bm{X}) \bm{X}^T = \bm{X} \bm{V}_k \bm{\Lambda} \bm{V}_k^T \bm{X}^T = (\bm{X} \bm{V}_k \bm{\Lambda}^{\frac{1}{2}})(\bm{X} \bm{V}_k \bm{\Lambda}^{\frac{1}{2}})^T \\

\bm{X} \bm{X}^T \bm{X} \bm{X}^T = (\bm{X} \bm{X}^T) (\bm{X} \bm{X}^T) = \bm{U}_k \bm{\Lambda} \bm{U}_k^T \bm{U}_k \bm{\Lambda} \bm{U}_k^T = \bm{U}_k \bm{\Lambda}^2 \bm{U}_k^T = (\bm{U}_k \bm{\Lambda})(\bm{U}_k \bm{\Lambda})^T \\

\therefore \bm{X} \bm{V}_k \bm{\Lambda}^{\frac{1}{2}} = \bm{U}_k \bm{\Lambda} \\
\therefore \bm{U}_k = \bm{X} \bm{V}_k \bm{\Lambda} ^ {-\frac{1}{2}}
$$