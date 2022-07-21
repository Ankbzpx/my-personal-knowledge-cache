
> Reference: https://mml-book.github.io/book/mml-book.pdf


For matrix $\bm{A} \in \mathbb{R}^{m \times n}$
$$
\bm{A} = \bm{U} \bm{\Sigma} \bm{V}^T
$$
where $\bm{U} \in \mathbb{R}^{m \times m}$, $\bm{V} \in \mathbb{R}^{n \times n}$ are orthogonal basis, $\bm{\Sigma} = diag\{\sigma_1, \sigma_2, \dots, \sigma_p\} \in \mathbb{R}^{m \times n}$, $p = \min\{m, n\}$. $\sigma_1 \geq \sigma_2 \geq \dots \geq \sigma_p \geq 0$ are singular values of $\bm{A}$

Define r as $\sigma_1 \geq \sigma_2 \geq \dots \geq \sigma_r \geq \sigma_{r+1} = \dots = \sigma_p = 0$, $rank(\bm{A}) = r$

$$
\bm{A} = \sum_{i=1}^{r} \sigma_i \bm{u}_i \bm{v}_i^T
$$

Full rank means $r = p$

## Thin SVD

If $m \geq n$
$$
\bm{A} = \bm{U}_1 \bm{\Sigma}_1 \bm{V}^T
$$
where $\bm{U}_1 \in \mathbb{R}^{m \times n}$, $\bm{\Sigma}_1 \in \mathbb{R}^{n \times n}$. Note that the basis are no longer orthogonal $$\bm{U}_1^T \bm{U}_1 = \bm{I} \in \mathbb{R}^{n \times n}$$, $\bm{U}_1 \bm{U}_1^T \neq \bm{I} \in \mathbb{R}^{m \times m}$

## Feature vector

Given $\bm{X} \in \mathbb{R}^{f \times n}$, $n \ll f$

Let $rank(\bm{X}) = k \leq \min(f, n) = n$

$$
\bm{X} \bm{X}^T = \bm{U} \bm{\Sigma} \bm{V}^T \bm{V} \bm{\Sigma} \bm{U}^T = \bm{U} \bm{\Sigma}^2 \bm{U}^T = \bm{U}_k \bm{\Sigma}_k^2 \bm{U}_k^T \in \mathbb{R}^{k \times k}
\\
\bm{X}^T \bm{X} = \bm{V} \bm{\Sigma} \bm{U}^T \bm{U} \bm{\Sigma} \bm{V}^T = \bm{V} \bm{\Sigma}^2 \bm{V}^T = \bm{V}_k \bm{\Sigma}_k^2 \bm{V}_k^T \in \mathbb{R}^{k \times k}
$$

It is equivalent to Eigen decompostion, where $\bm{\Lambda} = \bm{\Sigma}_k^2$, $\bm{U}_k \in \mathbb{R}^{f \times k}$, $\bm{V}_k \in \mathbb{R}^{n \times k}$

$$
\bm{X} \bm{X}^T = \bm{U}_k \bm{\Lambda} \bm{U}_k^T \\
\bm{X}^T \bm{X} = \bm{V}_k \bm{\Lambda} \bm{V}_k^T
$$

### Proof $\bm{X} \bm{X}^T$ and $\bm{X}^T \bm{X}$ share the same $\bm{\Lambda}$

Let $\bm{\Lambda} = \bm{\Sigma}_k^2$

Assume $\bm{v}$ is any eigenvector of $\bm{X}^T \bm{X}$ with non-zero eigenvalue $\lambda$

$$
\bm{X}^T \bm{X} \bm{v} = \lambda \bm{v} \\
\bm{X} \bm{X}^T \bm{X} \bm{v} = \lambda \bm{X} \bm{v} \\
(\bm{X} \bm{X}^T) \bm{X} \bm{v} = \lambda (\bm{X} \bm{v}) \\
\therefore \bm{X} \bm{v} \text{ is eigenvector of }\bm{X} \bm{X}^T \text{ corresponds to same eigenvalue } \lambda \\
\therefore \bm{X}^T \bm{X}, \bm{X} \bm{X}^T \text{ have the same } \bm{\Lambda}
$$

### Proof $\bm{U}_k = \bm{X} \bm{V}_k \bm{\Lambda} ^ {-\frac{1}{2}}$

$$
\bm{X} \bm{X}^T \bm{X} \bm{X}^T =\bm{X} (\bm{X}^T \bm{X}) \bm{X}^T = \bm{X} \bm{V}_k \bm{\Lambda} \bm{V}_k^T \bm{X}^T = (\bm{X} \bm{V}_k \bm{\Lambda}^{\frac{1}{2}})(\bm{X} \bm{V}_k \bm{\Lambda}^{\frac{1}{2}})^T \\

\bm{X} \bm{X}^T \bm{X} \bm{X}^T = (\bm{X} \bm{X}^T) (\bm{X} \bm{X}^T) = \bm{U}_k \bm{\Lambda} \bm{U}_k^T \bm{U}_k \bm{\Lambda} \bm{U}_k^T = \bm{U}_k \bm{\Lambda}^2 \bm{U}_k^T = (\bm{U}_k \bm{\Lambda})(\bm{U}_k \bm{\Lambda})^T \\

\therefore \bm{X} \bm{V}_k \bm{\Lambda}^{\frac{1}{2}} = \bm{U}_k \bm{\Lambda} \\
\therefore \bm{U}_k = \bm{X} \bm{V}_k \bm{\Lambda} ^ {-\frac{1}{2}}
$$