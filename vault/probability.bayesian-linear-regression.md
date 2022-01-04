---
id: fHMmzepyNqegNyO1rPHZJ
title: Bayesian Linear Regression
desc: ''
updated: 1641284360001
created: 1640605581119
---

Reference: https://mml-book.github.io

>FIXME: 1. Global Marco 2. Nested Alignment

## Problem formation
Let random variable $\pmb{x} \in \mathbb{R}^{D}$, parameter $\pmb{\theta} \in \mathbb{R}^{D}$, function value $f \in \mathbb{R}$ observed target value $y \in \mathbb{R}$

$$
y = f(\pmb{x}) + \epsilon = \pmb{x}^{T} \pmb{\theta} + \epsilon
$$
where $\epsilon \sim \mathcal{N}(0,\sigma^2)$ is independent identically distributed (i.i.d.) measurement noise.

### Probability density function (pdf)
$$
\int_{\mathbb{R}^{D}}p(\pmb{x})d\pmb{x}=1
$$

### Expected value & Covariance
#### Expected value
$$
\newcommand{\x}{\pmb{x}}
\newcommand{\y}{\pmb{y}}
\newcommand{\E}{\mathrm{E}}

\E_{\x}[\x] = \int\x p(\x) d\x \in \mathbb{R}^D \\

\E_{\x}[g(\x)] = \int g(\x) p(\x) d\x
$$

#### Covariance
Let $\pmb{x} \in \mathbb{R}^D$, $\pmb{y} \in \mathbb{R}^E$
$$
\newcommand{\x}{\pmb{x}}
\newcommand{\y}{\pmb{y}}
\newcommand{\E}{\mathrm{E}}
\newcommand{\Var}{\mathrm{Var}}
\newcommand{\Cov}{\mathrm{Cov}}

\Cov[\x, \y] = \pmb{\Sigma}_{\x \y} = \E_{\x, \y}[\x \y^T] - \E_{\x}[\x]\E_{\y}[\y]^T = {\Cov[\y, \x]}^T \in \mathbb{R}^{D \times E} \\

\Var[\x] = \pmb{\Sigma}_{\x \x} = \E_{\x}[(\x - \pmb{\mu})(\x - \pmb{\mu})^{T}] = \E_{\x}[\x\x^T] - \E_{\x}[\x]\E_{\x}[\x]^T \in \mathbb{R}^{D \times D}
$$

#### For dataset
> Monte-Carlo: random repeated sampling

Mean
$$
\pmb{\mu} = \frac{1}{N}\sum_{n=1}^{N}\pmb{x}_n \in \mathbb{R}^{D}
$$
Variance
$$
\pmb{\Sigma} = \frac{1}{N}\sum_{n=1}^{N}(\pmb{x}_n - \pmb{\mu})(\pmb{x}_n - \pmb{\mu})^T \in \mathbb{R}^{D \times D}
$$

#### Arithmetics
Let $\pmb{x} \in \mathbb{R}^D$, $\pmb{y} \in \mathbb{R}^D$

$$
\newcommand{\x}{\pmb{x}}
\newcommand{\y}{\pmb{y}}
\newcommand{\E}{\mathrm{E}}
\newcommand{\Var}{\mathrm{Var}}
\newcommand{\Cov}{\mathrm{Cov}}

\begin{aligned}
\E[\x+\y] &= \E[\x] + \E[\y] \\
\E[\x-\y] &= \E[\x] - \E[\y] \\
\Var[\x+\y] &= \Var[\x] + \Var[\y] - \Cov[\x, \y] + \Cov[\y, \x] \\
\Var[\x-\y] &= \Var[\x] + \Var[\y] - \Cov[\x, \y] - \Cov[\y, \x]
\end{aligned}
$$

Proof:
$$
\newcommand{\x}{\pmb{x}}
\newcommand{\y}{\pmb{y}}
\newcommand{\E}{\mathrm{E}}
\newcommand{\Var}{\mathrm{Var}}
\newcommand{\Cov}{\mathrm{Cov}}
\begin{aligned}
\Var[\x+\y] &= \E[(\x + \y)(\x + \y)^T] - \E[\x + \y]\E[\x + \y]^T \\
&= \E[\x\x^T] + \E[\x\y^T] + E[\y\x^T] + \E[\y\y^T] - (\E[\x]\E[\x]^T + \E[\x]\E[\y]^T + \E[\y]\E[\x]^T + \E[\y]\E[\y]^T) \\
&= (\E[\x\x^T] - \E[\x]\E[\x]^T) + (\E[\y\y^T] - \E[\y]\E[\y]^T) + (\E[\x\y^T] - \E[\x]\E[\y]^T) + (E[\y\x^T] - \E[\y]\E[\x]^T) \\
&= \Var[\x] + \Var[\y] + \Cov[\x, \y] + \Cov[\y, \x]
\end{aligned}
$$

#### Conditional
> Expect the value of a random variable, given its known value, to be that value
$$
\newcommand{\x}{\pmb{x}}
\newcommand{\E}{\mathrm{E}}
\newcommand{\Var}{\mathrm{Var}}

\E[\x|\x] = \x
\\
\Var[\x|\x] = \pmb{0}
$$

#### Affine transformation
Let $\pmb{x}$ be a random variable with mean $\pmb{\mu}$ and variance $\pmb{\Sigma}$, random variable $\pmb{y}$ as an affine transform of $\pmb{x}$ such that $\pmb{y} = \pmb{A}\pmb{x} + \pmb{b}$

$$
\newcommand{\x}{\pmb{x}}
\newcommand{\y}{\pmb{y}}
\newcommand{\A}{\pmb{A}}
\newcommand{\b}{\pmb{b}}
\newcommand{\E}{\mathrm{E}}
\newcommand{\Var}{\mathrm{Var}}
\newcommand{\Cov}{\mathrm{Cov}}

\begin{aligned}
\E_{\x}[\y] &= \E_{\x}[\A\x + \b] = \A \E_{\x}[\x] + \b = \A\pmb{\mu} = \b \\
\Var_{\y}[\y] &= \Var_{\x}[\A\x + \b] = \Var_{\x}[\A\x] = \A \Var_{\x} \A^T = \A \pmb{\Sigma} \A^T \\
\Cov[\x, \y] &= \pmb{\Sigma} \A^T
\end{aligned}
$$

Proof:
$$
\newcommand{\x}{\pmb{x}}
\newcommand{\y}{\pmb{y}}
\newcommand{\z}{\pmb{z}}
\newcommand{\A}{\pmb{A}}
\newcommand{\b}{\pmb{b}}
\newcommand{\E}{\mathrm{E}}
\newcommand{\Var}{\mathrm{Var}}
\newcommand{\Cov}{\mathrm{Cov}}

\begin{aligned}
\Var_{\x}[\A\x] &= \E_{\x}[\A\x(\A\x)^T] - \E[\A\x]\E[\A\x]^T \\
&= \E_{\x}[\A\x\x^T\A] - \A\E[\x](\A\E[\x])^T \\
&= \A \E[\x\x^T]\A^T - \A\E_{\x}[\x]\E_{\x}[\x]^T\A^T \\
&= \A (\E_{\x}[\x\x^T] - \E_{\x}[\x]\E_{\x}[\x]^T) \A^T \\
&= \A \Var_{\x} \A^T
\end{aligned} 

\\

\begin{aligned}
\Cov[\x + \y, \z] &= \E[(\x+\y)\z^T] + \E[\x + \y]\E[\z]^T \\ 
&= \E[\x\z^T] + \E[\y\z^T] + E[\x]\E[\z]^T + \E[\y]\E[\z]^T \\
&= (\E[\x\z^T] + E[\x]\E[\z]^T) + (\E[\y\z^T] + \E[\y]\E[\z]^T) \\
&= \Cov[\x, \z] + \Cov[\y, \z]
\end{aligned}

\\

\begin{aligned}
\Cov[\x, \y] &= \Cov[\x, \A\x + \b] \\ 
&= \Cov[\x, \A\x] + \Cov[\x, \b] \\ 
&= \Cov[\x, \A\x] \\
&= \Cov[\x, \x] \A^T \\
&= \pmb{\Sigma} \A^T
\end{aligned}
$$

### Sum rule / Marginalization 
$$
p(\pmb{x}) = \int p(\pmb{x}, \pmb{y})d\pmb{y}
$$

### Product rule
$$
p(\pmb{x}, \pmb{y}) = p(\pmb{y}|\pmb{x})p(\pmb{x})
$$

Combine sum rule and product rule
$$
p(\pmb{x}) = \int p(\pmb{x}|\pmb{y})p(\pmb{y})d\pmb{y}
$$

### Gaussian distribution
For $x \in \mathbb{R} \sim \mathcal{N}(\mu, \sigma)$

$$
p(x|\mu, \sigma) = \frac{1}{\sqrt{2\pi}\sigma}\exp{(-\frac{(x - \mu)^2}{2\sigma^2})}
$$

For $\pmb{x} \in \mathbb{R}^{D} \sim \mathcal{N}(\pmb{\mu}, \pmb{\Sigma})$

$$
p(\pmb{x} | \pmb{\mu}, \pmb{\Sigma}) = (2\pi)^{-\frac{D}{2}} (|\pmb{\Sigma}|)^{-\frac{1}{2}}\exp{(-\frac{1}{2}(\pmb{x} - \pmb{\mu})^{T}\pmb{\Sigma}^{-1}(\pmb{x} - \pmb{\mu}))} = \frac{1}{\sqrt{(2\pi)^D \pmb{\Sigma}}}\exp{(-\frac{1}{2}(\pmb{x} - \pmb{\mu})^{T}\pmb{\Sigma}^{-1}(\pmb{x} - \pmb{\mu}))}
$$

Consider joint distribution
$$
p(\pmb{x}, \pmb{y}) = \mathcal{N}(\begin{bmatrix} \pmb{\mu}_{\pmb{x}} \\ \pmb{\mu}_{\pmb{y}} \\ \end{bmatrix}, \begin{bmatrix} \pmb{\Sigma}_{\pmb{xx}} &\pmb{\Sigma}_{\pmb{xy}} \\ \pmb{\Sigma}_{\pmb{yx}} &\pmb{\Sigma}_{\pmb{yy}} \\ \end{bmatrix})
$$

#### Conditional
$$
\newcommand{\x}{\pmb{x}}
\newcommand{\y}{\pmb{y}}

p(\x|\y) = \mathcal{N}(\pmb{\mu}_{\x|\y}, \pmb{\Sigma}_{\x|\y}) \\
\pmb{\mu}_{\x|\y} = \pmb{\mu}_{\x} + \pmb{\Sigma}_{\x\y}\pmb{\Sigma}_{\y\y}^{-1}(\y - \pmb{\mu}_{\y}) \\
 \pmb{\Sigma}_{\x|\y} = \pmb{\Sigma}_{\x\x} - \pmb{\Sigma}_{\x\y}\pmb{\Sigma}_{\y\y}^{-1}\pmb{\Sigma}_{\y\x}
$$

Proof:
> Theorem: All conditional distributions of a multivariate normal distribution are normal
$$
\newcommand{\x}{\pmb{x}}
\newcommand{\y}{\pmb{y}}
\newcommand{\z}{\pmb{z}}
\newcommand{\A}{\pmb{A}}
\newcommand{\mux}{\pmb{\mu}_{\x}}
\newcommand{\muy}{\pmb{\mu}_{\y}}
\newcommand{\E}{\mathrm{E}}
\newcommand{\Cov}{\mathrm{Cov}}
\newcommand{\Var}{\mathrm{Var}}
\newcommand{\Sigmaxx}{\pmb{\Sigma}_{\x\x}}
\newcommand{\Sigmaxy}{\pmb{\Sigma}_{\x\y}}
\newcommand{\Sigmayx}{\pmb{\Sigma}_{\y\x}}
\newcommand{\Sigmayy}{\pmb{\Sigma}_{\y\y}}

\text{define} \ \z = \x + \A\y, \text{where} \ \A = - \Sigmaxy \Sigmayy^{-1}

\\

\begin{aligned}
\Cov[\z, \y] &= \Cov[\x + \A\y, \y] \\ &= \Cov[\x, \y] + \Cov[\A\y, \y] \\ &= \Sigmaxy + \A \Sigmayy \\ &= \Sigmaxy - \Sigmaxy \Sigmayy^{-1} \Sigmayy \\ &= \pmb{0}
\end{aligned}

\\
\begin{aligned}
\E[\x|\y] &= \E[\z - \A\y | \y] \\ &= \E[\z|\y] - \E[\A\y | \y] \\ &= \E[\z] - \A \y \\ &= \mux - \A(\y - \muy) \\ &= \mux + \Sigmaxy \Sigmayy^{-1}(\y - \muy)
\end{aligned}

\\
\begin{aligned}
\Var[\x|\y] &= \Var[\z - \A\y | \y] \\ &= \Var[\z | \y] + \Var[-\A\y | \y] + \Cov[\z|\y, - \A\y | \y] + \Cov[- \A\y | \y, \z|\y] \\ &= \Var[\z] \\ &= \Var[\x + \A\y] \\ &= \Var[\x] + \Var[\A\y] + \Cov[\x, \A\y] + \Cov[\A\y, \x] \\ &= \Sigmaxx + \A \Sigmayy \A^T + \A \Sigmaxy + \Sigmayx \A^T \\ &= \Sigmaxx + (- \Sigmaxy \Sigmayy^{-1}) \Sigmayy (- \Sigmaxy \Sigmayy^{-1})^T + (- \Sigmaxy \Sigmayy^{-1}) \Sigmaxy + \Sigmayx (- \Sigmaxy \Sigmayy^{-1})^T \\ &= \Sigmaxx + \Sigmaxy \Sigmayy^{-1} \Sigmayx - 2\Sigmaxy \Sigmayy^{-1}\Sigmayx \\ &= \Sigmaxx - \Sigmaxy \Sigmayy^{-1}\Sigmayx
\end{aligned}
$$

#### Marginal

#### Product

#### Linear transformation

#### Sampling with reparameterization trick

#### Sum of independent

### Bayes' theorem
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

