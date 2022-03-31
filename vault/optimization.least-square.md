---
id: 0901aqnyeeuh145frdq3auv
title: Least Square (LS)
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

## Least square approach

$$
y \sim \mathcal{N}(\bm{y}|\bm{X} \bm{\theta},\sigma^2 \bm{I})
\\
L = \frac{1}{2} \bm{r}^T\bm{r} = \frac{1}{2} (\bm{y} - \bm{X} \bm{\theta})^T (\bm{y} - \bm{X} \bm{\theta}) \in \mathbb{R}
\\
\frac{\partial L}{\partial \bm{\theta}} = -(\bm{y} - \bm{X} \bm{\theta})^T \bm{X} \in \mathbb{R}^{1 \times d}
$$

Set derivative to be 0

$$
\bm{\theta}^* = (\bm{X}^T \bm{X})^{-1} \bm{X}^T \bm{y} \in \mathbb{R}^d
$$

More see [[Maximum Likelihood Estimation (MLE)|probability.parameter-estimation.maximum-likelihood-estimation-mle]]