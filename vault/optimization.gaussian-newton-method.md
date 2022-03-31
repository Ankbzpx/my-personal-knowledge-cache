---
id: 2jcu9f9r56idl2xabx3izkk
title: Gaussian Newton Method
desc: ''
updated: 1648697815669
created: 1648697815669
---

Given objective function
$$
\argmin_x F(x) = e(x)^T e(x)
$$

Similar to [[optimization.newton-method]], we locally approximate its first order derivative and set it to 0

$$
\nabla F(x + \Delta x) = \nabla F(x) + \nabla^2F(x) \Delta x = 0
$$

Let $J = \frac{\partial e}{\partial x}$, $H = \frac{\partial^2 e}{\partial x^2}$
$$
\nabla F(x) = \frac{\partial F}{\partial x} = \frac{\partial F}{\partial e} \frac{\partial e}{\partial x} = 2 e^T(x)J
$$

$$
\nabla^2 F(x) = \frac{\partial^2 F}{\partial x^2} = \frac{\partial}{\partial x}(\frac{\partial F}{\partial e} \frac{\partial e}{\partial x}) = \frac{\partial 2 e^T(x)J}{\partial x} = 2(e^T(x)H + J^TJ) \approx 2J^TJ
$$

Thus we solve

$$
2 e^T(x)J + 2J^TJ \Delta x = 0
$$
Then set new guess
$$
x^* = x + \Delta x
$$