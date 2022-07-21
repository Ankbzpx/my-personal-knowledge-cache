
> Reference https://statweb.stanford.edu/~jtaylo/courses/stats203/notes/robust.pdf

$$
y \sim \mathcal{N}(\bm{y}|\bm{X} \bm{\theta},\sigma^2 \bm{W}^{-1})
$$

where diagonal weight matrix $\bm{W} \in \mathbb{R}^{n \times n}$

$$
L = \frac{1}{2} \bm{r}^T \bm{W}\bm{r} = \frac{1}{2} (\bm{y} - \bm{X} \bm{\theta})^T \bm{W}(\bm{y} - \bm{X} \bm{\theta}) \in \mathbb{R}

\\

\frac{\partial L}{\partial \bm{\theta}} = -(\bm{y} - \bm{X}\bm{\theta})^T\bm{W}\bm{X} \in \mathbb{R}^{1 \times d}
$$

Set derivative to be 0

$$
\bm{\theta}^* = (\bm{X}^T \bm{W} \bm{X})^{-1} \bm{X}^T \bm{W} \bm{y} \in \mathbb{R}^d
$$
