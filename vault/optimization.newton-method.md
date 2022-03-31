---
id: ww0ig1hc7dtcy7ormwfedch
title: Newton Method
desc: ''
updated: 1647624498444
created: 1647623137326
---

Approximate function value via first order derivative

## Root finding

$$
f(x_n) \approx f(x_{n-1}) + f'(x_{n-1})(x_n - x_{n-1}) = 0
\\
x_n = x_{n-1} - \frac{f(x_{n-1})}{f'(x_{n-1})}
$$

## Optimization

$$
f'(x_n) \approx f'(x_{n-1}) + f''(x_{n-1})(x_n - x_{n-1}) = 0 
\\
x_n = x_{n-1} - \frac{f'(x_{n-1})}{f''(x_{n-1})}
$$
