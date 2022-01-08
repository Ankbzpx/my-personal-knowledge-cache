---
id: LkOrm2ErP21wI6VSucSHZ
title: Bayesian Linear Regression
desc: ''
updated: 1641637509222
created: 1641632436928
---
Compute full posterior, make predictions by taking account of all possible model parameters.

## Train

With bayes' theorem
$$
p(\pmb{\theta}|\pmb{X}, \pmb{y}) = \frac{p(\pmb{y}|\pmb{X}, \pmb{\theta})p(\pmb{\theta})}{p(\pmb{y}|\pmb{X})}
$$

We have

$$
p(\pmb{\theta}|\pmb{X}, \pmb{y}) \propto p(\pmb{y}|\pmb{X}, \pmb{\theta})p(\pmb{\theta})
$$

Given likelihood $p(\pmb{y}|\pmb{X}, \pmb{\theta}) = \mathcal{N}(\pmb{y}|\pmb{\Phi}\pmb{\theta}, \sigma^2 \pmb{I})$, assuming Gaussian prior $p(\pmb{\theta}) = \mathcal{N}(\pmb{\theta}|\pmb{m}_0, \pmb{S}_0)$, $p(\pmb{\theta}|\pmb{X}, \pmb{y})$ is also Gaussian distributed. 

Assume $p(\pmb{\theta}|\pmb{X}, \pmb{y}) = \mathcal{N}(\pmb{\theta}|\pmb{m}_N, \pmb{S}_N)$, we have

$$
\mathcal{N}(\pmb{\theta}|\pmb{m}_N, \pmb{S}_N) \propto \mathcal{N}(\pmb{y}|\pmb{\Phi \theta}, \sigma^2 \pmb{I}) \mathcal{N}(\pmb{\theta}|\pmb{m}_0, \pmb{S}_0)
$$

Apply linear transform $\pmb{\theta} = (\pmb{\Phi}^T\pmb{\Phi})^{-1}\pmb{\Phi}^T\pmb{y}$ to transform Gaussian variable $\pmb{y}$ to $\pmb{\theta}$
$$
\begin{aligned}
\mathcal{N}(\pmb{y}|\pmb{\Phi \theta}, \sigma^2 \pmb{I}) &=
\mathcal{N}((\pmb{\Phi}^T\pmb{\Phi})^{-1}\pmb{\Phi}^T\pmb{y}|(\pmb{\Phi}^T\pmb{\Phi})^{-1}\pmb{\Phi}^T\pmb{\Phi}(\pmb{\Phi}^T\pmb{\Phi})^{-1}\pmb{\Phi}^T\pmb{y}, \sigma^2 (\pmb{\Phi}^T\pmb{\Phi})^{-1}\pmb{\Phi}^T((\pmb{\Phi}^T\pmb{\Phi})^{-1}\pmb{\Phi}^T)^T) \\ &=
\mathcal{N}(\pmb{\theta}|(\pmb{\Phi}^T\pmb{\Phi})^{-1}\pmb{\Phi}^T\pmb{y}, \sigma^2 (\pmb{\Phi}^T\pmb{\Phi})^{-1})
\end{aligned}
$$

Use [[Gaussian Product rule|probability.gaussian-distribution#product]]

$$
\pmb{S}_N = (\pmb{m}_0^{-1} + \sigma^{-2} (\pmb{\Phi}^T\pmb{\Phi}))^{-1}
\\
\pmb{m}_N = \pmb{S}_N(\pmb{S}_0^{-1} \pmb{m}_0 + \sigma^{-2}\pmb{\Phi}^T\pmb{y})
$$

## Inference
For new data, the predictive distribution
$$
y_* = \phi(\pmb{x_*})^T \pmb{\theta} + \epsilon
\\
p(y_*|\pmb{X}, \pmb{y}, \phi(\pmb{x_*})) = \int p(y_*|\phi(\pmb{x_*}), \pmb{\theta}) p(\pmb{\theta} | \pmb{X}, \pmb{y}) d \pmb{\theta} = \mathcal{N}(\phi(\pmb{x_*})^T\pmb{m}_N, \phi(\pmb{x_*})^T \pmb{S}_N\phi(\pmb{x_*}) + \sigma^2)
$$

> Predictive mean coincides with MAP

Integrate out $\pmb{\theta}$ induces the distribution of function

$$
f(.) \sim \mathcal{N}(\phi(.)^T\pmb{m}_N, \phi(.)^T \pmb{S}_N\phi(.))
$$
> $\phi(.)^T\pmb{m}_N$ is the mean function

By sampling $\pmb{\theta}_i \sim p(\pmb{\theta} | \pmb{X}, \pmb{y})$, we obtain a function realization $f_i(.) = \phi(.)^T\pmb{\theta}_i$