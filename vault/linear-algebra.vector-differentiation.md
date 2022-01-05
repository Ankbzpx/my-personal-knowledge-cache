---
id: eHYn1Q3sJsIn5EgXuqFbD
title: Vector Differentiation
desc: ''
updated: 1641356226750
created: 1641300273127
---

## Taylor Series

> Can be used to define operator such as matrix exponential and matrix logarithm

$$
f(x) = \sum_{k=0}^{\infin} \frac{f^{(k)}(x_0)}{k!}(x-x_0)^k
$$

### Taylor polynomial of degree n
Contains first $n+1$ components of the series.
>  Used as an approximation of function, at best when $x$ is close to $x_0$. Also an exact representation of polynominal of degree $k$ if $k \leq n$, as $f^{(i)}, i > k$ are all 0.

$$
T_n = \sum_{k=0}^{n} \frac{f^{(k)}(x_0)}{k!}(x-x_0)^k
$$

### Approximation
Gradient of $f$ at $x_0$ can be used for local approximation of $f$ around $x_0$
$$
f(x) \approx f(x_0) + f'(x_0)(x - x_0)
$$

## Vector differentiation

### Differentiation rules

> $\circ$ means composition of function

$$
\begin{aligned}
&\text{Product Rule:} \ \frac{\partial}{\partial } (fg)' = f'g + fg'
\\
&\text{Quotient Rule:} \ (\frac{f}{g})' = \frac{f'g - fg'}{g^2}
\\
&\text{Sum Rule:} \ (f + g)' = f' + g'
\\
&\text{Chain Rule:} \ (g(f))' = (g \circ f)' = g'(f)f'
\end{aligned}
$$

### Partial derivative
Let $f: \mathbb{R}^{n} \rarr \mathbb{R}$, $\pmb{x} \in \mathbb{R}^{n}$, $f(\pmb{x}) \in \mathbb{R}$:

$$
\nabla_{\pmb{x}}f = 
\begin{bmatrix}
\frac{\partial f(\pmb{x})}{\partial x_1} & \frac{\partial f(\pmb{x})}{\partial x_2} & \dots & \frac{\partial f(\pmb{x})}{\partial x_n} 
\end{bmatrix}
\in \mathbb{R}^{1 \times n}
$$

Let $f: \mathbb{R}^{n} \rarr \mathbb{R}^{m}$, $\pmb{x} \in \mathbb{R}^{n}$, $\pmb{f}(\pmb{x}) \in \mathbb{R}^{m}$:

$$
\nabla_{\pmb{x}}\pmb{f} = 
\begin{bmatrix}
\frac{\partial f_1(\pmb{x})}{\partial x_1} & \frac{\partial f_1(\pmb{x})}{\partial x_2} & \dots & \frac{\partial f_1(\pmb{x})}{\partial x_n} \\
\frac{\partial f_2(\pmb{x})}{\partial x_1} & \frac{\partial f_2(\pmb{x})}{\partial x_2} & \dots & \frac{\partial f_2(\pmb{x})}{\partial x_n} \\
\vdots & & & \vdots \\
\frac{\partial f_m(\pmb{x})}{\partial x_1} & \frac{\partial f_m(\pmb{x})}{\partial x_2} & \dots & \frac{\partial f_m(\pmb{x})}{\partial x_n}
\end{bmatrix}
\in \mathbb{R}^{m \times n}
$$

$\nabla_{\pmb{x}}\pmb{f}$ is called the Jacobian $J$

### Basic rules
$$
\newcommand{\x}{\pmb{x}}
\newcommand{\s}{\pmb{s}}
\newcommand{\a}{\pmb{a}}
\newcommand{\b}{\pmb{b}}
\newcommand{\A}{\pmb{A}}
\newcommand{\W}{\pmb{W}}
\newcommand{\X}{\pmb{X}}
\newcommand{\fX}{f(\pmb{X})}
\newcommand{\finvX}{f^{-1}(\pmb{X})}
\newcommand{\d}{\partial}

\begin{aligned}
&\frac{\d}{\d\X}\fX^T = (\frac{\d}{\d \X}\fX)^T \\
&\frac{\d}{\d\X}tr(\fX) = tr(\frac{\d}{\d \X}\fX) \\
&\frac{\d}{\d\X}det(\fX) = det(\fX)tr(\finvX\frac{\d}{\d \X} \fX) \\
&\frac{\d}{\d\X}\finvX = - \finvX \frac{\d}{\d \X} \fX \finvX \\
&\frac{\d}{\d\X} \a^T \X \b = -(\X^{-1})^T\a\b^T(\X^{-1})^T \\
&\frac{\d}{\d\x}\x^T \a = \a^T \\
&\frac{\d}{\d\x}\a^T \x = \a^T \\
&\frac{\d}{\d\x}\x^T \A \x = 2 \x^T \A \\
&\frac{\d}{\d\s}(\x - \A\s)^T\W(\x - \A\s) = -2(\x - \A\s)^T \W \A \\
\end{aligned}
$$