---
id: r5t0c0o2i8n2r4to50z8614
title: Partial Derivative (Jacobian Matrix)
desc: ''
updated: 1648695554903
created: 1648695554903
---


Let $f: \mathbb{R}^{n} \rarr \mathbb{R}$, $\pmb{x} \in \mathbb{R}^{n}$, $f(\pmb{x}) \in \mathbb{R}$:

$$ \bm{J} =
\nabla_{\pmb{x}}f = 
\begin{bmatrix}
\frac{\partial f(\pmb{x})}{\partial x_1} & \frac{\partial f(\pmb{x})}{\partial x_2} & \dots & \frac{\partial f(\pmb{x})}{\partial x_n} 
\end{bmatrix}
\in \mathbb{R}^{1 \times n}
$$

Let $f: \mathbb{R}^{n} \rarr \mathbb{R}^{m}$, $\pmb{x} \in \mathbb{R}^{n}$, $\pmb{f}(\pmb{x}) \in \mathbb{R}^{m}$:

$$
\bm{J} = 
\nabla_{\pmb{x}}\pmb{f} = 
\begin{bmatrix}
\frac{\partial f_1(\pmb{x})}{\partial x_1} & \frac{\partial f_1(\pmb{x})}{\partial x_2} & \dots & \frac{\partial f_1(\pmb{x})}{\partial x_n} \\
\frac{\partial f_2(\pmb{x})}{\partial x_1} & \frac{\partial f_2(\pmb{x})}{\partial x_2} & \dots & \frac{\partial f_2(\pmb{x})}{\partial x_n} \\
\vdots & & & \vdots \\
\frac{\partial f_m(\pmb{x})}{\partial x_1} & \frac{\partial f_m(\pmb{x})}{\partial x_2} & \dots & \frac{\partial f_m(\pmb{x})}{\partial x_n}
\end{bmatrix}
\in \mathbb{R}^{m \times n}
$$

$\bm{J}$ is known as the Jacobian Matrix