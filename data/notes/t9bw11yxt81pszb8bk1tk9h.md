
> Reference: https://www.ams.org/journals/mcom/1981-37-155/S0025-5718-1981-0616367-1/S0025-5718-1981-0616367-1.pdf

Perform weighted least square at each point $\bm{x} \in \bm{X}$ locally ([[Polynomial basis|probability.parameter-estimation.maximum-likelihood-estimation-mle#polynomial-basis]], over $k$ neighbourhood points within a distance threshold, with weight as a function of distance).

$$
\argmin_{\bm{\theta}_{\bm{x}}} \sum_{i=1}^{k} \theta(\| \bm{x}_i - \bm{x} \|_2) (\bm{y}_i - \phi(\bm{x}_i)^T \bm{\theta}_{\bm{x}})^2
$$

where $\phi(.)$ is multivariate polynomial basis function, $\theta$ is weight function, $\bm{\theta}_{\bm{x}}$ is parameter to be estimated at $\bm{x}$

> **IMPORTANT** This method is different from previous method, as it needs $\bm{\theta}_{\bm{x}_i}$ for each $\bm{x}_i$, so $n$ in total.

For test data, find $\bm{\theta}$ by interpolating neighbourhood $\bm{\theta}_{\bm{x}}$ weighted by same function of distance (interpolation needs weights normalized).

## Derivatives computation

For each sample $\bm{x}_i \in \bm{X}$ with $k$ neighbour points $\bm{X}_i = \begin{bmatrix} \bm{x}_0 & \dots & \bm{x}_j & \dots & \bm{x}_k \end{bmatrix}^T$. Given $\bm{y}_i = \begin{bmatrix} y_0 & \dots & y_k \end{bmatrix}^T$, $\bm{\Phi}_i = \begin{bmatrix} \phi(\bm{x}_0) & \dots & \phi(\bm{x}_k) \end{bmatrix}^T$; weight $\bm{W}_i = diag(\theta(\|\bm{x}_i - \bm{x}_j\|))$

The total gradient is:

$$

\frac{\partial L}{\bm{\partial \theta}} = \begin{bmatrix} \frac{\partial L}{\partial \bm{\theta}_0}^T & \dots & \frac{\partial L}{\partial \bm{\theta}_i}^T & \dots & \frac{\partial L}{\partial \bm{\theta}_n}^T \end{bmatrix} \in \mathbb{R}^{d \times n}
$$

where 

$$
\frac{\partial L}{\partial \bm{\theta}_i} = -(\bm{y}_i - \bm{\Phi}_i\bm{\theta}_i)^T\bm{W}_i\bm{\Phi}_i
$$

## Wendland function
> Reference: https://www.researchgate.net/publication/240320304_Geometric_transformation_of_the_RBF_implicit_surface

Compactly-supported Radius basis function $\phi_k$ for smoothness parameter $k=0,1,2$

$$
\phi(r)_0 = (1 - r)_+^2 \\
\phi(r)_1 = (1 - r)_+^4(4r + 1) \\
\phi(r)_2 = (1 - r)_+^6(35r^2 + 18r + 3)
$$

where
$$
(1 - r)_+ =

\begin{cases}
1 - r & r < 1 \\
0     & \text{otherwise}
\end{cases}

$$

### Radial Basis function
> Related: [[Kernel Trick|linear-algebra.feature-extraction.principle-component-analysis#kernel-trick]]

> Reference https://arxiv.org/pdf/1203.5696.pdf

Function $\Phi: \mathbb{R}^d \rightarrow \mathbb{R}$ is radial if $\Phi(\bm{x}) = \phi(\|\bm{x}\|_2)$ for all $\bm{x} \in \mathbb{R}^d$. Radial Basis Function (RBF) can be defined for a given center $\bm{x}_i \in \mathbb{R}^d$ as
$$
\Phi_i(\bm{x}) = \phi(\| \bm{x} - \bm{x}_i \|_2)
$$
The function values depends only on the distance between input $\bm{x}$ and center $\bm{x}_i$S


RBFs can be categorised as:

- **Global-supported basis function**: consider all distance
- **Compactly-supported basis function**: ignore point pairs beyond a predefined distance
