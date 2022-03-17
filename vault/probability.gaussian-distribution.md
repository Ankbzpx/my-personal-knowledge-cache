---
id: S02aBkyPu6J4lH9rhcG26
title: Gaussian Distribution
desc: ''
updated: 1647454148124
created: 1641285365685
---

>FIXME: 1. Global Marco 2. Nested Alignment

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

## Conditional
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

## Marginal
$$
\newcommand{\x}{\pmb{x}}
\newcommand{\y}{\pmb{y}}
\newcommand{\mux}{\pmb{\mu}_{\x}}
\newcommand{\Sigmaxx}{\pmb{\Sigma}_{\x\x}}

p(\x) = \int p(\x, \y) d\y = \mathcal{N}(\x | \mux, \Sigmaxx)
$$

## Product
The product of two Gaussians $\mathcal{N}(\pmb{x} | \pmb{a}, \pmb{A})$$\mathcal{N}(\pmb{x} | \pmb{b}, \pmb{B})$ is an unnormalized Gaussian $\mathcal{N}(\pmb{x} | \pmb{c}, \pmb{C})$ with
$$
\pmb{C} = (\pmb{A}^{-1} + \pmb{B}^{-1})^{-1}
\\
\pmb{c} = \pmb{C}(\pmb{A}^{-1} \pmb{a} + \pmb{B}^{-1} \pmb{b})
$$

Proof:
$$
\newcommand{\a}{\pmb{a}}
\newcommand{\b}{\pmb{b}}
\newcommand{\x}{\pmb{x}}
\newcommand{\A}{\pmb{A}}
\newcommand{\B}{\pmb{B}}
\newcommand{\C}{\pmb{C}}

\begin{aligned}
\text{Apply log transformation} \ &\log \mathcal{N}(\x|\a, \A) \mathcal{N}(\x|\b, \B) \\ = & -\frac{1}{2}((\x - \a)^T \A^{-1}(\x - \a) + (\x - \b)^T \B^{-1}(\x - \b)) + const \\
= & -\frac{1}{2}(\x^T \A^{-1} \x - 2 \a^T \A^{-1} \x + \a^T \A^{-1} \a + \x^T \B^{-1} \x - 2 \b^T \B^{-1} \x + \b^T \B^{-1} \b) + const \\ = & -\frac{1}{2}(\x^T (\A^{-1} + \B^{-1}) \x - 2(\A^{-1} \a + \B^{-1} \b)^T\x) + const
\end{aligned}

\\

\begin{aligned}
\text{We know that} \ & \exp(\log \mathcal{N}(\x | \pmb{c}, \C)) = \exp(-\frac{1}{2}((\x - \pmb{c})^T \C^{-1}(\x - \pmb{c}) + const) \\ \propto & \exp(-\frac{1}{2}(\x^T \C^{-1} \x - 2 (\C^{-1} \pmb{c})^T \x)) \\ \propto & \exp(\log \mathcal{N}(\x|\a, \A) \mathcal{N}(\x|\b, \B)) \\ \propto & -\frac{1}{2}(\x^T (\A^{-1} + \B^{-1}) \x - 2(\A^{-1} \a + \B^{-1} \b)^T\x)
\end{aligned}

\\
\begin{aligned}
\text{By completing the square, we have} \ & \C = (\A^{-1} + \B^{-1})^{-1}
\\& \pmb{c} = \C(\A^{-1} \a + \B^{-1} \b)
\end{aligned}
$$

The normalizing constant is
$$
c = (2\pi)^{-\frac{D}{2}}|\pmb{A} + \pmb{B}|^{\frac{1}{2}}\exp{(-\frac{1}{2}(\pmb{a}-\pmb{b})^T(\pmb{A} + \pmb{B})^{-1}(\pmb{a}-\pmb{b}))}
$$

## Linear transformation
> Linear transformation of Gaussian variable is Gaussian distributed

Let $p(\pmb{x}) = \mathcal{N}(\pmb{x} | \pmb{\mu}, \pmb{\Sigma})$, $\pmb{y} = \pmb{A} \pmb{x}$, then
$$
\pmb{y} = \mathcal{N}(\pmb{y} | \pmb{A} \pmb{\mu}, \pmb{A} \pmb{\Sigma} \pmb{A}^T)
$$

Let $p(\pmb{y}|\pmb{x}) = \mathcal{N}(\pmb{y}|\pmb{A}\pmb{x}, \pmb{\Sigma})$, then
$$
\pmb{x} = (\pmb{A}^T \pmb{A})^{-1}\pmb{A}^T\pmb{y}

\\

p(\pmb{x}|\pmb{y}) = \mathcal{N}((\pmb{A}^T \pmb{A})^{-1}\pmb{A}^T\pmb{y}, (\pmb{A}^T \pmb{A})^{-1}\pmb{A}^T \pmb{\Sigma} \pmb{A}(\pmb{A}^T \pmb{A})^{-1})
$$

## Sampling with reparameterization trick
Instead of sampling from $\pmb{y} \sim \mathcal{N}(\pmb{\mu}, \pmb{\Sigma})$, sampling from $\pmb{x} \sim \mathcal{N}(\pmb{0}, \pmb{I})$, find $\pmb{A}\pmb{A}^T = \pmb{\Sigma}$, then $\pmb{y} = \pmb{A}\pmb{x} + \pmb{\mu}$

## Sum of independent
> Sum of independent Gaussian variables is also Gaussian distributed

$$
p(\pmb{x} + \pmb{y}) = \mathcal{N}(\pmb{\mu}_{\pmb{x}} + \pmb{\mu}_{\pmb{y}}, \pmb{\Sigma}_{\pmb{x}} + \pmb{\Sigma}_{\pmb{y}})
$$