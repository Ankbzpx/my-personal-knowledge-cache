---
id: ww0ig1hc7dtcy7ormwfedch
title: Newton
desc: ''
updated: 1647624498444
created: 1647623137326
---

## Newton Method

### Root finding

$$
f(x_n) \approx f(x_{n-1}) + f'(x_{n-1})(x_n - x_{n-1}) = 0
\\
x_n = x_{n-1} - \frac{f(x_{n-1})}{f'(x_{n-1})}
$$

### Optimization

$$
f'(x_n) \approx f'(x_{n-1}) + f''(x_{n-1})(x_n - x_{n-1}) = 0 
\\
x_n = x_{n-1} - \frac{f'(x_{n-1})}{f''(x_{n-1})}
$$

## Gaussian-Newton Method
Given objective function
$$
\argmin_x F(x) = e(x)^T e(x)
$$

We compute its first order derivative and set it be 0

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