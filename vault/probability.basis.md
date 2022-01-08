---
id: ZAYUjF6BZWBn7m6NPt8Dn
title: Basis
desc: ''
updated: 1641632398417
created: 1641285249620
---
## Probability density function (pdf)
$$
\int_{\mathbb{R}^{D}}p(\pmb{x})d\pmb{x}=1
$$

## Sum rule / Marginalization 
$$
p(\pmb{x}) = \int p(\pmb{x}, \pmb{y})d\pmb{y}
$$

## Product rule
$$
p(\pmb{x}, \pmb{y}) = p(\pmb{y}|\pmb{x})p(\pmb{x})
$$

Combine sum rule and product rule
$$
p(\pmb{x}) = \int p(\pmb{x}|\pmb{y})p(\pmb{y})d\pmb{y}
$$

## Bayes' theorem
$$
p(\pmb{\theta}|\pmb{x}, y) = \frac{p(y|\pmb{x}, \pmb{\theta})p(\pmb{\theta})}{p(y|\pmb{x})}
$$

$$
posterior = \frac{likelihood \times prior}{marginal\ likelihood}
$$

Proof with product rule
$$
p(y|\pmb{x}, \pmb{\theta})p(\pmb{\theta}) = p(\pmb{\theta}, y|\pmb{x})
$$

$$
p(\pmb{\theta}|\pmb{x}, y)p(y|\pmb{x}) = p(\pmb{\theta}, y|\pmb{x})
$$

Also with sum rule
> Marginal likelihood is the likelihood averaged over all possible $\pmb{\theta}$

$$
p(y|\pmb{x}) = \int p(y|\pmb{x}, \pmb{\theta})p(\pmb{\theta})d\theta
$$