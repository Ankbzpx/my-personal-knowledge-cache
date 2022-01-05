---
id: fHMmzepyNqegNyO1rPHZJ
title: Parameter Estimation
desc: ''
updated: 1641299570302
created: 1640605581119
---

Reference: https://mml-book.github.io

## Problem formation
Let random variable $\pmb{x} \in \mathbb{R}^{D}$, parameter $\pmb{\theta} \in \mathbb{R}^{D}$, function value $f \in \mathbb{R}$ observed target value $y \in \mathbb{R}$

$$
y = f(\pmb{x}) + \epsilon = \pmb{x}^{T} \pmb{\theta} + \epsilon
$$
where $\epsilon \sim \mathcal{N}(0,\sigma^2)$ is independent identically distributed (i.i.d.) measurement noise.
$$
y \sim \mathcal{N}(y|\pmb{x}^{T} \pmb{\theta},\sigma^2)
$$

![[Bayes' theorem|probability.basis#bayes-theorem]]
