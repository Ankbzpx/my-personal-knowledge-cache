---
id: gae0o4cc0yvkjbxtufeqssf
title: Inner Product
desc: ''
updated: 1648779392404
created: 1648779392404
---
> Reference: https://mml-book.github.io/book/mml-book.pdf

## Definition

Let $V$ be a vector space, the inner product on $V$ is positive definite, symmetrix bilinear mapping to real number
$$
\langle.,.\rangle: V \times V \to \mathbb{R}
$$

### Symmetric
$$
\forall \bm{x}, \bm{y} \in V:
\Omega(\bm{x}, \bm{y}) = \Omega(\bm{y}, \bm{x})
$$

### Positive definite
$$
\forall \bm{x} \in V \backslash \{\bm{0}\} : \Omega(\bm{x}, \bm{x}) > 0 \\
\Omega(\bm{0}, \bm{0}) = 0
$$

### Bilinear
Linear mapping for two arguments
$$
\Omega(\lambda \bm{x} + \phi \bm{y}, \bm{z}) = \lambda \Omega(\bm{x}, \bm{z}) + \phi \Omega(\bm{y}, \bm{z}) \\
\Omega(\bm{x}, \lambda \bm{y} + \phi \bm{z}) = \lambda \Omega(\bm{x}, \bm{y}) + \phi \Omega(\bm{x}, \bm{z})
$$

## Symmetric, Positive definite matrix
$\forall \bm{x}, \bm{y} \in V$ can be written as linear combination of basis vectors $B = (b_1, \dots b_n)$. 

$$\bm{x} = \sum_{i=1}^{n} \phi_i \bm{b}_i \\
\bm{y} = \sum_{i=1}^{n} \lambda_i \bm{b}_i$$

Thus the inner product

$$
\langle\bm{x},\bm{y}\rangle = \sum_{i=1}^{n} \sum_{i=1}^{n} \phi_i \langle\bm{b}_i,\bm{b}_j\rangle \lambda_i = \hat{\bm{x}}^T \bm{A} \hat{\bm{y}}^T
$$

By definition $\bm{A}$ is symmetric

### Symmetric, Positive definite
$$
\forall \bm{x} \in V \backslash \{\bm{0}\}: \bm{x}^T \bm{A} \bm{x} >0
$$

All positive eigenvalue

### Symmetric, Positive semidefinite

$$
\forall \bm{x} \in V \backslash \{\bm{0}\}: \bm{x}^T \bm{A} \bm{x} \geq0
$$

Non-negative eigenvalue

## Length
The length of vector $\bm{x}$

$$
\| \bm{x} \| := \sqrt{\langle \bm{x}, \bm{x} \rangle}
$$

It satisfies Cauchy-Schwarz inequality
$$
|\langle \bm{x}, \bm{y} \rangle| \leq \| \bm{x} \| \| \bm{y} \|
$$

## Integration form
> Reference: https://math.stackexchange.com/questions/1143886/the-integral-form-of-inner-product

Inner product of continuous function f, g on $x \in \Omega$
$$
\langle f, g \rangle = \int_\Omega f(x) g(x) dx
$$