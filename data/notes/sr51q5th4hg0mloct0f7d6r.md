
> Reference: https://mml-book.github.io/book/mml-book.pdf

Goal: center data by subtracting mean


Given $\bm{X} = \begin{bmatrix} \bm{x}_0, \dots, \bm{x}_n \end{bmatrix} \in \mathbb{R}^{f \times n}$, $n \ll f$

### Vector form
 See [[Expected value and covariance for dataset|probability.expected-value-and-covariance#for-dataset]]
 
### Matrix form

Note we assume $\bm{X} \in \mathbb{R}^{f \times n}$

#### Mean

$\bm{1}$ vector can be used to sum of axis

$$
\bm{m} = \frac{1}{n} \bm{X} \bm{1}_n \in \mathbb{R}^f\\

\bm{M} = (\frac{1}{n}\bm{X} \bm{1}_n) \bm{1}_n^T \in \mathbb{R}^{f \times n} \\

\overline{\bm{X}} = \bm{X} - \bm{M} = \bm{X}(\bm{I} - \frac{1}{n}\bm{1}_n\bm{1}_n^T)
$$

## Covariance
$$
\bm{S}_t = \frac{1}{n} \overline{\bm{X}} \overline{\bm{X}}^T
$$
