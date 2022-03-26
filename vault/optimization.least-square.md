---
id: 0901aqnyeeuh145frdq3auv
title: Least Square
desc: ''
updated: 1648002918750
created: 1647599853388
---

> Reference: http://www.nealen.net/projects/mls/asapmls.pdf

Under the assumption of gaussian likelihood.
Random variable $\bm{X} \in \mathbb{R}^{n \times d}$, parameter $\bm{\theta} \in \mathbb{R}^{d}$, observation $\bm{y} \in \mathbb{R}^{n}$, the residual
$$
\bm{r} = \bm{y} - \bm{X} \bm{\theta} \in \mathbb{R}^n
$$
## Least square (LS)

$$
y \sim \mathcal{N}(\bm{y}|\bm{X} \bm{\theta},\sigma^2 \bm{I})
\\
L = \frac{1}{2} \bm{r}^T\bm{r} = \frac{1}{2} (\bm{y} - \bm{X} \bm{\theta})^T (\bm{y} - \bm{X} \bm{\theta}) \in \mathbb{R}
\\
\frac{\partial L}{\partial \bm{\theta}} = (\bm{y} - \bm{X} \bm{\theta})^T \bm{X} \in \mathbb{R}^{1 \times d}
$$

Set derivative to be 0

$$
\bm{\theta}^* = (\bm{X}^T \bm{X})^{-1} \bm{X}^T \bm{y} \in \mathbb{R}^d
$$

More see [[Maximum Likelihood Estimation (MLE)|probability.parameter-estimation.maximum-likelihood-estimation-mle]]

## Weighted least square (WLS)

> Reference https://statweb.stanford.edu/~jtaylo/courses/stats203/notes/robust.pdf

$$
y \sim \mathcal{N}(\bm{y}|\bm{X} \bm{\theta},\sigma^2 \bm{W}^{-1})
$$

where diagonal weight matrix $\bm{W} \in \mathbb{R}^{n \times n}$

$$
L = \frac{1}{2} \bm{r}^T \bm{W}\bm{r} = \frac{1}{2} (\bm{y} - \bm{X} \bm{\theta})^T \bm{W}(\bm{y} - \bm{X} \bm{\theta}) \in \mathbb{R}

\\

\frac{\partial L}{\partial \bm{\theta}} = (\bm{y} - \bm{X}\bm{\theta})^T\bm{W}\bm{X} \in \mathbb{R}^{1 \times d}
$$

Set derivative to be 0

$$
\bm{\theta}^* = (\bm{X}^T \bm{W} \bm{X})^{-1} \bm{X}^T \bm{W} \bm{y} \in \mathbb{R}^d
$$

## Iteratively reweighted least square (IRLS)
> Reference: http://sepwww.stanford.edu/data/media/public/docs/sep115/jun1/paper_html/node2.html

Choose $diag(\bm{W}) = |\bm{r}|^{(p-2)}$, so that the loss function
$$
\bm{r}^T \bm{W} \bm{r} = \bm{r}^T |\bm{r}|^{p-2} \bm{r} = \bm{r}^p
$$
which minimize the $l_p$ norm of the residual (usually $1 \le p \le 2$)

### Steps
1. Start with $diag(\bm{W}) = \bm{1}$
2. Compute weighted least square fit and residual $\bm{r}$
3. Adjust weight $diag(\bm{W}) = |\bm{r}|^{(p-2)/2}$
4. Re-fit and repeat for few iterations

## Moving least square (MLS)

> Reference: https://www.ams.org/journals/mcom/1981-37-155/S0025-5718-1981-0616367-1/S0025-5718-1981-0616367-1.pdf

Perform weighted least square at each point $\bm{x} \in \bm{X}$ locally ([[Polynomial basis|probability.parameter-estimation.maximum-likelihood-estimation-mle#polynomial-basis]], over $k$ neighbourhood points within a distance threshold, with weight as a function of distance).

$$
\argmin_{\bm{\theta}_{\bm{x}}} \sum_{i=1}^{k} \theta(\| \bm{x}_i - \bm{x} \|_2) (\bm{y}_i - \phi(\bm{x}_i)^T \bm{\theta}_{\bm{x}})^2
$$

where $\phi(.)$ is multivariate polynomial basis function, $\theta$ is weight function, $\bm{\theta}_{\bm{x}}$ is parameter to be estimated at $\bm{x}$

> **IMPORTANT** This method is different from previous method, as it needs $\bm{\theta}_{\bm{x}_i}$ for each $\bm{x}_i$, so $n$ in total.

For test data, find $\bm{\theta}$ by interpolating neighbourhood $\bm{\theta}_{\bm{x}}$ weighted by same function of distance (interpolation needs weights normalized).


### Wendland function
> Reference: https://www.researchgate.net/publication/240320304_Geometric_transformation_of_the_RBF_implicit_surface

Compactly-supported Radius basis function $\phi_k$ for smoothness parameter $k=0,1,2$

$$
\phi(r)_0 = (1 - r)_+^2 \\
\phi(r)_1 = (1 - r)_+^4(4r + 1) \\
\phi(r)_2 = (1 - r)_+^6(35r^2 + 18r + 3)
$$

where
$$
(1 - r)_+ =

\begin{cases}
1 - r & r < 1 \\
0     & \text{otherwise}
\end{cases}

$$

#### Radial Basis function
> Related: [[Kernel Trick|linear-algebra.feature-extraction#kernel-trick]]

> Reference https://arxiv.org/pdf/1203.5696.pdf

Function $\Phi: \mathbb{R}^d \rightarrow \mathbb{R}$ is radial if $\Phi(\bm{x}) = \phi(\|\bm{x}\|_2)$ for all $\bm{x} \in \mathbb{R}^d$. Radial Basis Function (RBF) can be defined for a given center $\bm{x}_i \in \mathbb{R}^d$ as
$$
\Phi_i(\bm{x}) = \phi(\| \bm{x} - \bm{x}_i \|_2)
$$
The function values depends only on the distance between input $\bm{x}$ and center $\bm{x}_i$S


RBFs can be categorised as:

- **Global-supported basis function**: consider all distance
- **Compactly-supported basis function**: ignore point pairs beyond a predefined distance