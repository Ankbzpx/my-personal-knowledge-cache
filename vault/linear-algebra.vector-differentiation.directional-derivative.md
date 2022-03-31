---
id: t8ancl9webs77qg1glqn1it
title: Directional Derivative
desc: ''
updated: 1648695819655
created: 1648695819655
---

> Reference: https://www.deeplearningbook.org/contents/numerical.html

Rate of change of function value in the direction of $\bm{u}$. 

$$
\nabla _{\bm{u}} f = \nabla_{\bm{x}} f \cdot \frac{\bm{u}}{\|\bm{u}\|}
$$

It is the derivative of $f(\bm{x}+ \alpha\bm{u})$ w.r.t. $\alpha$, evaluated at $\alpha$.

Let $\bm{y} = \bm{x} + \alpha \bm{u}$, with chain rule

$$
\frac{\partial f}{\partial \alpha} = \frac{\partial f}{\partial y} \frac{\partial y}{\partial \alpha} = \nabla_{\bm{x}} f \cdot \bm{u}
$$
