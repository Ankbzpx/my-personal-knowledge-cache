---
id: 0901aqnyeeuh145frdq3auv
title: Least Square
desc: ''
updated: 1647623051185
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