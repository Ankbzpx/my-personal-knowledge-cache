---
id: 0901aqnyeeuh145frdq3auv
title: Least Square
desc: ''
updated: 1647916993476
created: 1647599853388
---

Under the assumption of gaussian likelihood.
Random variable $\bm{X} \in \mathbb{R}^{n \times d}$, parameter $\bm{\theta} \in \mathbb{R}^{d}$, observation $\bm{y} \in \mathbb{R}^{n}$, the residual
$$
\bm{r} = \bm{y} - \bm{X} \bm{\theta} \in \mathbb{R}^n
$$
## Least square (LS)

$$
y \sim \mathcal{N}(\bm{y}|\bm{X} \bm{\theta},\sigma^2 \bm{I})
\\
L = \bm{r}^T\bm{r} = (\bm{y} - \bm{X} \bm{\theta})^T (\bm{y} - \bm{X} \bm{\theta})
$$

More see [[Maximum Likelihood Estimation (MLE)|probability.parameter-estimation.maximum-likelihood-estimation-mle]]

## Weighted least square (WLS)

> Reference https://statweb.stanford.edu/~jtaylo/courses/stats203/notes/robust.pdf

$$
y \sim \mathcal{N}(\bm{y}|\bm{X} \bm{\theta},\sigma^2 \bm{W}^{-2})
$$

where diagonal weight matrix $\bm{W} \in \mathbb{R}^{n \times n}$

$$
L = (\bm{W}\bm{r})^T\bm{W}\bm{r} = \bm{r}^T \bm{W}^T\bm{W}\bm{r} = (\bm{y} - \bm{X} \bm{\theta})^T \bm{W}^T \bm{W}(\bm{y} - \bm{X} \bm{\theta})

\\

\frac{\partial L}{\partial \bm{\theta}} = 2(\bm{y} - \bm{X}\bm{\theta})^T\bm{W}^T\bm{W}\bm{X} = 2(\bm{W}(\bm{y} - \bm{X}\bm{\theta}))^T\bm{W}\bm{X} = 2(\bm{W}\bm{r})^T\bm{W}\bm{X}
$$

## Iteratively reweighted least square (IRLS)
> Reference: http://sepwww.stanford.edu/data/media/public/docs/sep115/jun1/paper_html/node2.html

Choose $diag(\bm{W}) = |\bm{r}|^{(p-2)/2}$, so that the loss function
$$
\bm{r}^T \bm{W}^T\bm{W}\bm{r} = \bm{r}^T |\bm{r}|^{p-2} \bm{r} = \bm{r}^p
$$
which minimize the $l_p$ norm of the residual (usually $1 \le p \le 2$)

### Steps
1. Start with $diag(\bm{W}) = \bm{1}$
2. Compute weighted least square fit and residual $\bm{r}$
3. Adjust weight $diag(\bm{W}) = |\bm{r}|^{(p-2)/2}$
4. Re-fit and repeat for few iterations

## Moving least square
Weighted least square of polynomial basis with weights depend on a set of sample points evaluted at input points


### Wendland function
> Reference: https://www.researchgate.net/publication/240320304_Geometric_transformation_of_the_RBF_implicit_surface

Compactly-supported Radius basis function $\phi_k$ for $k=0,1,2$

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