---
id: 6s6bsmshdd3rc0la4ow3hu4
title: Triangulation
desc: ''
updated: 1652427580628
created: 1652420121637
---
> Reference: https://www.robots.ox.ac.uk/~vgg/hzbook/

> Note it's under the assumption that error only occur in image, while the projection transformation is accurate

![](/assets/images/2022-05-13-13-36-43.png){width: 250px}

Start from reference frame (left), let its projection matrix be $\bm{P}$,

$$
\bm{x} = (\alpha)\bm{P} \bm{X}
$$

which is valid up to a scaling factor. To eliminate scale ambiguity, we use cross product
$$
\bm{x} \times \bm{P} \bm{X} = \bm{0}
$$

Assume $\bm{P} = \begin{bmatrix}
\bm{p}_1^T \\
\bm{p}_2^T \\
\bm{p}_3^T
\end{bmatrix} \in \mathbb{R}^{3 \times 4}$, where $\bm{p}_i^T$, $\bm{p}_2^T$, $\bm{p}_3^T$ are rows of $\bm{P}$, we have

$$
\bm{P} \bm{X} = \begin{bmatrix}
\bm{p}_1^T \bm{X} \\
\bm{p}_2^T \bm{X} \\
\bm{p}_3^T \bm{X} \\
\end{bmatrix}
$$

Given $\bm{x} = (x, y, 1)^T$
$$
\begin{aligned}
\bm{x} \times \bm{P} \bm{X} &=
\begin{bmatrix}
0 & -1 & y \\
1 & 0 & -x \\
-y & x & 0
\end{bmatrix}
\begin{bmatrix}
\bm{p}_1^T \bm{X} \\
\bm{p}_2^T \bm{X} \\
\bm{p}_3^T \bm{X} \\
\end{bmatrix} \\ &=
\begin{bmatrix}
y \bm{p}_3^T \bm{X} - \bm{p}_2^T \bm{X} \\
\bm{p}_1^T \bm{X} - x \bm{p}_3^T \bm{X} \\
x \bm{p}_2^T \bm{X} - y \bm{p}_1^T \bm{X} \\
\end{bmatrix} = 
\begin{bmatrix}
0 \\
0 \\
0
\end{bmatrix}
\end{aligned}
$$

Thus with two equations (the third line is a linear combination of first two)

$$
y \bm{p}_3^T \bm{X} - \bm{p}_2^T \bm{X} = 0 \\
\bm{p}_1^T \bm{X} - x \bm{p}_3^T \bm{X} = 0
$$

with matrix form

$$
\begin{bmatrix}
y \bm{p}_3^T - \bm{p}_2^T \\
\bm{p}_1^T - x \bm{p}_3^T
\end{bmatrix} \bm{X} = \bm{0}
$$

Take current frame into consideration, we have

$$
\begin{bmatrix}
y \bm{p}_3^T - \bm{p}_2^T \\
\bm{p}_1^T - x \bm{p}_3^T \\
y' \bm{p}_3'^T - \bm{p}_2'^T \\
\bm{p}_1'^T - x' \bm{p}_3'^T
\end{bmatrix} \bm{X} = \bm{0}
$$

that can be solved with [[optimization.least-square]] (3 unknowns with 4 equations)