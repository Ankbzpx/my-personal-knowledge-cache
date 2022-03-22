---
id: XdjIVWbNYspG1sYd6jURu
title: Maximum Likelihood Estimation (MLE)
desc: ''
updated: 1647916690514
created: 1641624854478
---
> prone to overfitting

## Basic

Find the optimal parameter $\pmb{\theta}$ by maximizing the likelihood function $p(\pmb{y}|\pmb{X}, \pmb{\theta})$.
> Likelihood is a distribution of $\pmb{y}$, a function of $\pmb{\theta}$

$$
\newcommand{\y}{\pmb{y}}
\newcommand{\X}{\pmb{X}}
\newcommand{\x}{\pmb{x}}

\begin{aligned}
\pmb{\theta}^* &= \underset{\pmb{\theta}}{\argmax} \ p(\y|\X, \pmb{\theta}) \\ &= \underset{\pmb{\theta}}{\argmin} (- \log p(\y|\X, \pmb{\theta})) \\ &= \underset{\pmb{\theta}}{\argmin} - (\log \prod_{i=0}^{N} p(y_i|{\x}_i, \pmb{\theta})) \\ &= \underset{\pmb{\theta}}{\argmin} \sum_{i=1}^{N} - \log p(y_i|{\x}_i, \pmb{\theta})
\end{aligned}
$$

We know $y \sim \mathcal{N}(y|\pmb{x}^{T} \pmb{\theta},\sigma^2)$ such that:

$$
\newcommand{\x}{\pmb{x}}

- \log p(y_i|{\x}_i, \pmb{\theta}) = \frac{1}{2\sigma^2}(y_i - \x^{T} \pmb{\theta})^2 + const
$$

The loss function (negative log likelihood) can be defined in Vector form:

$$
\newcommand{\y}{\pmb{y}}
\newcommand{\X}{\pmb{X}}

L(\pmb{\theta}) = \frac{1}{2\sigma^2} (\y - \X\pmb{\theta})^T(\y - \X\pmb{\theta})
$$

As a quadratic term of $\pmb{\theta}$, the global minimum can be computed by setting gradient to 0:

$$
\begin{aligned}
\frac{\partial L}{ \partial \pmb{\theta}} &= -\frac{1}{\sigma^2} (\pmb{y} - \pmb{X}\pmb{\theta})^T\pmb{X} = \pmb{0}
\\
\pmb{\theta}^* &= (\pmb{X}^T\pmb{X})^{-1}\pmb{X}^T \pmb{y}
\end{aligned}
$$

## With Features
Let nonlinear transformation $\phi: \mathbb{R}^D \rightarrow \mathbb{R}^K$, $\pmb{\theta} \in \mathbb{R}^K$
$$
y = \phi^T(\pmb{x})\pmb{\theta} + \epsilon
$$

In vector form:
$$
\pmb{\Phi} = \pmb{\phi}(\pmb{x}) = \begin{bmatrix} \phi_1(\pmb{x})& \phi_2(\pmb{x}) \dots \phi_k(\pmb{x}) \end{bmatrix}^T \in \mathbb{R}^K
$$

With close form solution:

$$
\pmb{\theta}^* = (\pmb{\Phi}^T\pmb{\Phi})^{-1}\pmb{\Phi}^T \pmb{y}
$$

### Polynomial basis
For $x \in \mathbb{R}$

$$
\phi(\bm{x}) = \begin{bmatrix} 1 \\ x \\ x^2 \\ \vdots \\ x^k \end{bmatrix} \in \mathbb{R}^{k+1}
$$

For $\bm{x} \in \mathbb{R}^d$, multivariate polynomial

> Reference: https://www.dbs.ifi.lmu.de/Lehre/MaschLernen/SS2017/Skript/03-BasisFunctions2017.pdf

$$
\bm{x} = \begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix} \in \mathbb{R}^3
$$
Its multivariate polynomial basis of degree 2 (with all permutation of elements in $\bm{x}$)

$$
\phi(\bm{x}) = \begin{bmatrix} 1 \\ x_1 \\ x_2 \\ x_3 \\ x_1 x_2 \\ x_2 x_3 \\ x_1 x_3 \\ x_1^2 \\ x_2^2 \\ x_3^2 \end{bmatrix} \in \mathbb{R}^{10}
$$

## Regularization

$$
L(\pmb{\theta}) =  -\log p(\pmb{y}|\pmb{X}, \pmb{\theta}) + \lambda \|\pmb{\theta}\|_2^2
$$