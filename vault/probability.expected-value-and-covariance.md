---
id: f02Y1nt8nJzX4RjchUruZ
title: Expected Value and Covariance
desc: ''
updated: 1641285634005
created: 1641285469505
---
>FIXME: 1. Global Marco 2. Nested Alignment

## Expected value
$$
\newcommand{\x}{\pmb{x}}
\newcommand{\y}{\pmb{y}}
\newcommand{\E}{\mathrm{E}}

\E_{\x}[\x] = \int\x p(\x) d\x \in \mathbb{R}^D \\

\E_{\x}[g(\x)] = \int g(\x) p(\x) d\x
$$

## Covariance
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

## For dataset
> Monte-Carlo: random repeated sampling

Mean
$$
\pmb{\mu} = \frac{1}{N}\sum_{n=1}^{N}\pmb{x}_n \in \mathbb{R}^{D}
$$
Variance
$$
\pmb{\Sigma} = \frac{1}{N}\sum_{n=1}^{N}(\pmb{x}_n - \pmb{\mu})(\pmb{x}_n - \pmb{\mu})^T \in \mathbb{R}^{D \times D}
$$

## Arithmetics
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

## Conditional
> Expect the value of a random variable, given its known value, to be that value
$$
\newcommand{\x}{\pmb{x}}
\newcommand{\E}{\mathrm{E}}
\newcommand{\Var}{\mathrm{Var}}

\E[\x|\x] = \x
\\
\Var[\x|\x] = \pmb{0}
$$

## Affine transformation
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
