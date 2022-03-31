---
id: niios4bi198zm3rsf45bdbe
title: Taylor Series
desc: ''
updated: 1648695409661
created: 1648695409661
---

$$
f(x) = \sum_{k=0}^{\infin} \frac{f^{(k)}(x_0)}{k!}(x-x_0)^k
$$

> It can be used to define operator such as matrix exponential and matrix logarithm


## Taylor polynomial of degree n
Contains first $n+1$ components of the series.
>  Used as an approximation of function, at best when $x$ is close to $x_0$. Also an exact representation of polynominal of degree $k$ if $k \leq n$, as $f^{(i)}, i > k$ are all 0.

$$
T_n = \sum_{k=0}^{n} \frac{f^{(k)}(x_0)}{k!}(x-x_0)^k
$$

## Local approximation
Gradient of $f$ at $x_0$ can be used for local approximation of $f$ around $x_0$
$$
f(x) \approx f(x_0) + f'(x_0)(x - x_0) + \frac{1}{2} f''(x_0)(x - x_0)^2
$$

Application see [[optimization.gaussian-newton-method]]
