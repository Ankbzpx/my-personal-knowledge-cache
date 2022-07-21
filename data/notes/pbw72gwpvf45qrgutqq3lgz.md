
> Reference: https://www.math-linux.com/mathematics/linear-systems/article/cholesky-decomposition

Symmetric positive definite $\bm{A}$ can be decomposed into $\bm{A} = \bm{L}\bm{L}^T$, where $\bm{L}$ is lower triangle matrix

## Application
To solve $\bm{A} \bm{x} = \bm{b}$, first solve $\bm{L} \bm{y} = \bm{b}$ then $\bm{y} = \bm{L}^T \bm{x}$. Note linear system with lower/upper triangle matrix can be easily solve with [[forward substitution|linear-algebra.cholesky-decomposition#forward-substitution]] / [[backward substitution|linear-algebra.cholesky-decomposition#backward-substitution]]

### forward substitution
> Reference: https://en.wikipedia.org/wiki/Triangular_matrix#:~:text=In%20the%20mathematical%20discipline%20of,the%20main%20diagonal%20are%20zero.

For lower triangle matrix system $\bm{L} \bm{x} = \bm{b}$, in linear equation form, we have

$$
\begin{aligned}
&l_{1, 1} x_1 &= b_1 \\
&l_{2, 1} x_1 + l_{2, 2} x_2 &= b_2 \\
&l_{3, 1} x_1 + l_{3, 2} x_2 + l_{3, 3} x_3 &= b_3 \\
&\dots \\
&l_{m, 1} x_1 + l_{m, 2} x_2 + l_{m, 3} x_3 + \dots + l_{m, m} x_m &= b_m
\end{aligned}
$$
$x_i$ can be solved recursively with $x_{i-1}$

### backward substitution
Similar to [[forward substitution|linear-algebra.cholesky-decomposition#forward-substitution]] but going backwards