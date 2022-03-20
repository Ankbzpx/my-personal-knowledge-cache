---
id: sr51q5th4hg0mloct0f7d6r
title: Feature Extraction
desc: ''
updated: 1647758366517
created: 1647411862801
---

> Reference: https://mml-book.github.io/book/mml-book.pdf


Given $\bm{X} = \begin{bmatrix} \bm{x}_0, \dots, \bm{x}_n \end{bmatrix} \in \mathbb{R}^{f \times n}$, $n \ll f$

## Data centering
Center data by subtracting mean
### Vector form
 See [[Expected value and covariance for dataset|probability.expected-value-and-covariance#for-dataset]]
 
### Matrix form

Note we assume $\bm{X} \in \mathbb{R}^{f \times n}$

#### Mean

$\bm{1}$ vector can be used to sum of axis

$$
\bm{m} = \frac{1}{n} \bm{X} \bm{1}_n \in \mathbb{R}^f\\

\bm{M} = (\frac{1}{n}\bm{X} \bm{1}_n) \bm{1}_n^T \in \mathbb{R}^{f \times n} \\

\overline{\bm{X}} = \bm{X} - \bm{M} = \bm{X}(\bm{I} - \frac{1}{n}\bm{1}_n\bm{1}_n^T)
$$

### Covariance
$$
\bm{S}_t = \frac{1}{n} \overline{\bm{X}} \overline{\bm{X}}^T
$$

## Principle Component Analysis

Find a set of basis $\bm{W} = \begin{bmatrix} \bm{w}_1, \dots, \bm{w}_k \end{bmatrix}^T \in \mathbb{R}^{f \times d}$, such that variance of the linear projection of $\bm{x}_i$ on $\bm{w}$ is maximized. See [[Dot product as projection|linear-algebra.basis#geometry]]

### Brief Derivation

$$
\bm{y}_i = \bm{W}^T \bm{x}_i \\

\sigma_{\bm{Y}} = \frac{1}{n} \sum_{i=1}^{n} (\bm{y}_i - \mu_{\bm{y}})(\bm{y}_i - \mu_{\bm{y}})^T =  \frac{1}{n} \sum_{i=1}^{n} \bm{W}^T \overline{\bm{x}}_i \overline{\bm{x}}_i^T \bm{W} = tr(\bm{W}^T \bm{S}_t \bm{W}) \\
$$
So for objective function:
$$
\begin{aligned}
\bm{W}_o &= \argmax_{\bm{W}} \sigma_{\bm{y}} \\
&= \argmax_{\bm{W}} tr(\bm{W}^T \bm{S}_t \bm{W})
\end{aligned}
$$
We incorperate with constraint $\bm{W}^T \bm{W} = \bm{I}$ and fomulate with Lagrangian:
$$
L(\bm{W}, \bm{\Lambda}) = tr(\bm{W}^T \bm{S}_t \bm{W}) - tr(\bm{\Lambda}(\bm{W}^T \bm{W} - \bm{I}))
$$

Compute its derivative over $\bm{W}$ and set it to 0. See: [[Basic rules|linear-algebra.vector-differentiation#basic-rules]]. Note $\bm{S}_t$ is positive semi-definte matrix (non-negative eigenvalues), $\bm{\Lambda}$ is diagonal matrix.

$$
\frac{\partial L}{\partial \bm{W}} = tr(2\bm{W}^T \bm{S}_t) - tr(2 \bm{\Lambda} \bm{W}^T) = 0 \\

\bm{S}_t \bm{W} = \bm{W} \bm{\Lambda} \\
$$

$\bm{S}_t \bm{w}_k = \lambda_k \bm{w}_k$ for $i=1,\dots,k$. Thus, $\bm{W}$ column-wise are $d$ eignvectors of $\bm{S}_t$. 

From [[Eigen decomposition|linear-algebra.decomposition#eigen-decomposition]], we have $\bm{S}_t = \bm{U} \bm{\Lambda} \bm{U}^T$, $\bm{U}_d = \bm{W}$. Go back to objective function

$$
tr(\bm{W}^T \bm{S}_t \bm{W}) = tr(\bm{W}^T \bm{U} \bm{\Lambda} \bm{U}^T \bm{W}) = tr(\bm{\Lambda}_d) = \sum_{k=1}^{d} \lambda_k
$$

We choose $d$ eignvectors that corresponds to $d$ largest eigenvalues

### Computation

For $\bm{X} \in \mathbb{R}^{f \times n}$, $n \ll f$, $\bm{X}\bm{X}^T \in \mathbb{R}^{f \times f}$ is more computational expensive to compute than $\bm{X}^T \bm{X} \in \mathbb{R}^{n \times n}$.

Since they [[share the same eigenvalues|linear-algebra.decomposition#proof-bmx-bmxt-and-bmxt-bmx-share-the-same-bmlambda]], we eigendecompose $\bm{X}^T \bm{X} = \bm{V} \bm{\Lambda} \bm{V}^T$ and compute $\bm{U} = \bm{X} \bm{V} \bm{\Lambda} ^ {-\frac{1}{2}}$. See [[proof|linear-algebra.decomposition#proof-bmu_k--bmx-bmv_k-bmlambda---frac12]].

We pick $d$ eigenvectors (columns of $\bm{U}$) and projected feature
$$
\bm{Y} = \bm{U}_d^T \bm{X}
$$

Note the covariance of projected feature
$$
\bm{Y} \bm{Y}^T = \bm{U}_d^T \bm{X} \bm{X}^T \bm{U}_d = \bm{\Lambda}_d
$$
which means projected features are uncorrelated

#### Whiten
If we wish $\bm{Y} \bm{Y}^T = \bm{I}$
$$
\bm{W} = \bm{U}_d \bm{\Lambda}_d^{-\frac{1}{2}}
$$

### Kernel Trick
Suppose $f$ is very large (e.g. infinity) that is intractable, but $\bm{X}^T \bm{X}$ is solvable
$$
\bm{X}^T \bm{X} = 
\begin{bmatrix} 
k(x_1, x_1) & \dots & k(x_1, x_n) \\
\vdots & \ddots & \\
k(x_n, x_1) &        & k(x_n, x_n) 
\end{bmatrix}
\in \mathbb{R}^{n \times n}
$$
We can still compute PCA

$$
\bm{X}^T \bm{X} = \bm{V} \bm{\Lambda} \bm{V}^T
$$

$$
\bm{Y} = \bm{U}_d^T \bm{X} = (\bm{X} \bm{V} \bm{\Lambda} ^ {-\frac{1}{2}})^T \bm{X} = \bm{V}^T \bm{\Lambda} ^ {-\frac{1}{2}} (\bm{X}^T \bm{X})
$$

During inference, for test sample $\bm{x}_t \in \mathbb{R}^{f \times 1}$

$$
y_t = \bm{V}^T \bm{\Lambda} ^ {-\frac{1}{2}} (\bm{X}^T \bm{x}_t)
$$

where

$$
\bm{X}^T \bm{x}_t = 
\begin{bmatrix}
k(x_1, x_t) \\
\vdots \\
k(x_n, x_t)
\end{bmatrix}
$$
