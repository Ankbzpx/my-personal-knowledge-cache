> Reference: https://mathworld.wolfram.com/TotalDerivative.html

Given function $f(x(t), y(t), z(y))$, its total derivative is:

$$
\frac{\partial f}{\partial t} =
\frac{\partial f}{\partial x}
\frac{\partial x}{\partial t} + 
\frac{\partial f}{\partial y}
\frac{\partial y}{\partial t} + 
\frac{\partial f}{\partial z}
\frac{\partial z}{\partial t}
$$

Total derivative $\frac{\partial f}{\partial t}(x_0, y_0, z_0)$ can be interpreted as [[linear-algebra.vector-differentiation.directional-derivative]] evaluated at $\begin{bmatrix} x(t_0) & y(t_0) & z(t_0) \end{bmatrix}^T$ with the direction of $\begin{bmatrix} x'(t_0) & y'(t_0) & z'(t_0) \end{bmatrix}^T$

