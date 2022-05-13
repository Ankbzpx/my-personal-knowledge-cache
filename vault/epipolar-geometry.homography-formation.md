---
id: 9tj97gvvzufmew51pkjv3zf
title: Homography Formation
desc: ''
updated: 1652419940278
created: 1652321247487
---

> Reference: https://www.robots.ox.ac.uk/~vgg/hzbook/

## Derivation
Given tbe diagram of reference frame (left) and current frame (right)
![](/assets/images/2022-05-12-10-09-24.png){width: 300px}

- $\bm{\pi}$ is the reference plane, with norm $\bm{n}$
- $\bm{c}$, $\bm{c'}$ are camera centers
- $d$, $d'$ are distances from camera centers to reference plane respectively
- $\bm{x}_{\pi}$ is landmark on $\bm{\pi}$ in 3D space
- $\bm{x}$, $\bm{x}'$ are respective projections of $\bm{x}_{\pi}$ on image planes

Assume coordinate origin at $\bm{c}$, the plane $\bm{\pi}$ can be represented by 
$$
\bm{n}^T \bm{x} + d = 0
$$

Let camera intrinsic matrix $\bm{K}$, from projective transform, we have
$$
\bm{x} = \bm{K} [\bm{I}|\bm{0}] \bm{x}_{\pi}
$$
Let $\bm{K}^{-1} \bm{x} = \bm{\hat{x}} = (x, y, 1)^T$, $\bm{x}_{\pi}$ lies on the ray, so $\bm{x}_{\pi} = (x, y, 1, \rho)^T = (\bm{\hat{x}}, \rho)^T$. since it also lies on the plane $\bm{\pi}$, therefore
$$
\bm{n}^T \bm{\hat{x}} + d \rho = 0 \\
\rho = -\frac{\bm{n}^T}{d} \bm{\hat{x}}
\\
\therefore \bm{x}_{\pi} = (\bm{\hat{x}}, -\frac{\bm{n}^T}{d} \bm{\hat{x}})^T
$$
Assume camera $\bm{c}$ (reference frame) has the transform $[\bm{R} | \bm{t}]$ w.r.t. $\bm{c}'$ (current frame) ($\bm{T}_{CR}$), let $\bm{\hat{x}'} = \bm{K}^{-1} \bm{x}'$, we have

> Note $[\bm{R} | \bm{t}] \bm{x}_{\pi}$ transforms $\bm{x}_{\pi}$ in reference frame to current frame

$$
\bm{\hat{x}'} = [\bm{R} | \bm{t}] \bm{x}_{\pi} = (\bm{R} - \bm{t} \frac{\bm{n}^T}{d}) \bm{\hat{x}}
$$
Thus, $\bm{\hat{x}'} = \bm{H} \bm{\hat{x}}$, $\bm{x}' = \bm{KHK}^{-1} \bm{x}$ where $\bm{H} = \bm{R} - \bm{t} \frac{\bm{n}^T}{d}$ is the Homography Matrix

> Note $\bm{H}$ is in Euclidean space while $\bm{KHK}^{-1}$ is in projective space

## Fitting
Homography Matrix $\bm{H} \in \mathbb{R}^{3 \times 3}$ has 8 DoF ($H_{33} = 1$) that can be fit with 4 pairs of coplanar points.

### Robust fitting
using
[[optimization.least-square]] with Ransac, or [[optimization.least-square.iteratively-reweighted-least-square]]

## Decomposition
Homography Matrix $\bm{H} = \bm{R} - \bm{t} \frac{\bm{n}^T}{d}$ can be decomposed into rotation $\bm{R}$, normalized translation $\frac{\bm{t}}{d}$ and plane normal $\bm{n}$

### Solution ambiguity
> Reference: https://hal.inria.fr/inria-00174036/PDF/RR-6303.pdf

Homography decomposition usually has 8 solutions

#### Non crossing contraints
4 of 8 can be filtered by checking if two cameraa are on the opposite of plane $\bm{\pi}$
$$
\frac{\bm{t}}{d}^T \bm{n} < d'
$$

#### Point visibility constraint
2 of remaining 4 can be filtered by checking if unprojected image points are in front of the plane
$$
\bm{\hat{x}}^T \bm{n} > 0
\\
\bm{\hat{x}'}^T (\bm{R} \bm{n}) > 0
$$

Find the most suitable one from the two needs more information, such as the knowledge of camera $z$ axis and plane normal