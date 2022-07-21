> Regularize with prior $p(\pmb{\theta})$

Find the optimal parameter $\pmb{\theta}$ by maximizing the posterior function $p(\pmb{\theta}|\pmb{X}, \pmb{y})$

$$
p(\pmb{\theta}|\pmb{X}, \pmb{y}) = \frac{p(\pmb{y}|\pmb{X}, \pmb{\theta})p(\pmb{\theta})}{p(\pmb{y}|\pmb{X})}
$$

Apply negative log transform:
> Marginal likelihood is not a function of $\pmb{\theta}$

$$
-\log p(\pmb{\theta}|\pmb{X}, \pmb{y}) = - \log p(\pmb{y}|\pmb{X}, \pmb{\theta}) - \log p(\pmb{\theta}) + const
$$

Assume gaussian prior $p(\pmb{\theta}) = \mathcal{N}(\pmb{0}, b^2 \pmb{I})$, $b^2 = \frac{1}{2 \lambda}$

$$
- \log p(\pmb{\theta}) = \frac{1}{2b^2}\pmb{\theta}^T \pmb{\theta} = \lambda \|\pmb{\theta}\|_2^2
$$

It is equivalent to [[MLE with Regularization|probability.parameter-estimation.maximum-likelihood-estimation-mle#regularization]].

The negative log posterior loss function:

$$
L(\pmb{\theta}) = \frac{1}{2\sigma^2} (\pmb{y} - \pmb{X}\pmb{\theta})^T(\pmb{y} - \pmb{X}\pmb{\theta}) + \frac{1}{2b^2}\pmb{\theta}^T \pmb{\theta} + const
$$

Set gradient to 0:

$$
\frac{\partial L}{\partial \pmb{\theta}} = - \frac{1}{\sigma^2}(\pmb{y} - \pmb{X}\pmb{\theta})^T\pmb{X} - \frac{1}{b^2}\pmb{\theta}^T = 0

\\
\pmb{\theta} = (\pmb{X}^T \pmb{X} - \frac{\sigma^2}{b^2} \pmb{I})\pmb{X}^T \pmb{y}

$$