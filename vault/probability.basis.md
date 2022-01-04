---
id: ZAYUjF6BZWBn7m6NPt8Dn
title: Basis
desc: ''
updated: 1641285519219
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
p(\pmb{\theta}|\pmb{x}, \pmb{y}) = \frac{p(\pmb{y}|\pmb{x}, \pmb{\theta})p(\pmb{\theta})}{p(\pmb{y}|\pmb{x})}
$$

$$
posterior = \frac{likelihood \times prior}{marginal\ likelihood}
$$

Proof with product rule
$$
p(\pmb{y}|\pmb{x}, \pmb{\theta})p(\pmb{\theta}) = p(\pmb{\theta}, \pmb{y}|\pmb{x})
$$

$$
p(\pmb{\theta}|\pmb{x}, \pmb{y})p(\pmb{y}|\pmb{x}) = p(\pmb{\theta}, \pmb{y}|\pmb{x})
$$

Also with sum rule
$$
p(\pmb{y}|\pmb{x}) = \int p(\pmb{y}|\pmb{x}, \pmb{\theta})p(\pmb{\theta})d\theta
$$