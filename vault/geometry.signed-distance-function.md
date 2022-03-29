---
id: wdxd75nh2cs2qklj06exvmx
title: Signed Distance Function
desc: ''
updated: 1647760187980
created: 1647755447905
---

Signed distance function is a function defined in $\bm{x} \in \mathbb{R}^3$, such that the function value $f(\bm{x})$  equals to the signed distance from point $\bm{x}$ to the closest surface. To fit it, we need:

- A set of surface points $\bm{x}_i \in \mathbb{R}^3$ (zero-level set $f(\bm{x}_i) = 0$)
- Normal vectors $\bm{n}_i \in \mathbb{R}^3$ for those points

## Point cloud normal estimation

> Reference: https://cs.nyu.edu/~panozzo/gp/04%20-%20Normal%20Estimation,%20Curves.pdf

To assign normal vector $\bm{n}$ for point $\bm{x}$, we need
- Estimate direction by fitting local plane
- Find consistent global orientation

### Find local plane
> Reference: https://www.graphics.rwth-aachen.de/media/papers/p_Pau021.pdf

For each point $\bm{x}$, we pick the $n-1$ nearest neighbour points. The objective is to fit a plane for these $n$ points, such that the sum of distance for each point to the plane is minimized.  The distance of point to surface can be modeled as [[Dot product|linear-algebra.basis#dot-product]] with surface normal $\bm{n}$, given a point $\bm{c}$ the plane pass through. This problem can be solved with [[Least square (LS)|optimization.least-square#least-square-ls]]

$$
min \sum_{i=1}^{n} ((\bm{x}_i - \bm{c})^T\bm{n})^2
$$

Note that it is equivalent to [[Principle Component Analysis|linear-algebra.feature-extraction#principle-component-analysis]], by performing eigenanalysis on the covariance matrix ($\mathrm{Cov} \in \mathbb{R}^{3 \times 3}$) of those points. We pick $(\pm)\bm{n}$ as the eigenvector that corresponds the smallest eigenvalue (We want to minimized the variance as opposed to PCA)

### Find consistent global orientation
#### Viewpoint known
Orient all normals consistently towards viewpoint
#### Viewpoint unknown
See http://mediatum.ub.tum.de/doc/800632/941254.pdf

## Gradient of iso-surface

For iso-surface $S$ defined by $f(\bm{x}) = 0$, the total derivate $df|_{\bm{p}} = 0$ for a point $\bm{p} \in S$. Given the definition of total derivative, we have

$$
df|_{\bm{p}} = \nabla f(\bm{p}) d\bm{p} = 0
$$

By definition $\nabla f(\bm{p})$ is perpendicular to tangent plane at $\bm{p}$. Thus, normal of $S$ at point $\bm{p} \in S$ is proportional to $\nabla f(\bm{p})$.

## Function fitting with constraints

### Spatial Index
- Uniform grid
- RTree: hierarchy of aabbs (Python example see: [[polygon.connect-polygons-with-their-offset-ones]])
- K-d tree: binary division of whole space

### Build constraints
Compute non zero-level data samples $f(\bm{x}_i) \neq 0$

> May cause undesired effects when distance of evaluation point to surface is much larger than offset distance

Inward / outward offsets each surface points along its normals for a distance such that the closest point of the offset point is the original point

### Surface fiting
Fit with [[Moving least square (MLS)|optimization.least-square#moving-least-square-mls]]

### Inference
- Build grid samples points
- Perform [[Principle Component Analysis|linear-algebra.feature-extraction#principle-component-analysis]] on grid samples points' **convex hull**
- Rotate grid samples points based on priciple axis
- Inference (weighted sum of neighourhood fitted parameters, with weight depends on distance) for each grid samples
- Apply [[code-read.igl.marching-cubes]]

## Function fitting with normal
> Reference: https://people.eecs.berkeley.edu/~jrs/papers/cartons.pdf

- Each point was paired with a function, which define the signed distance to this points' tangent plane
- Fit and interpolate MLS using that function value


## TODO
- [x] Fir point cloud normals
- [x] Moving least square
- [ ] Why use polynomial basis?
- [x] Marching cube
- [ ] Screened Poisson
- [ ] RIMLS
