---
id: d5w6tbfyq2jp8e1lpmqi6fy
title: Subdivision
desc: ''
updated: 1647505824570
created: 1647315756405
---
> Reference: https://www.graphics.rwth-aachen.de/media/papers/sqrt31.pdf

## Concepts
### Valence
Number of incident edges a.k.a. vertex degree

### Extraordinary Vertices
Vertices where the mesh is not regular, e.g. valence 3 or 5 for quad mesh

###  $C^1$ $C^2$ Smoothness
Continuous first / second order derivatives. For mesh is derivative of its limit surface

### n-adic Split
Intersect n new vertices for all edges

Dy-adic split: bi-sects, 4 subfaces
Tri-adic split: tri-sects, 9 subfaces

### Permutation matrix
Square binary matrix, exactly one entry of 1 each row and each column and 0s elsewhere

## Basic idea
- Use coarse control surface instead of parametric surface
- Refine control surface that eventually converge to a smooth limit surface

### Operator
- Split: introduce new vertex
- Smoothing: change old vertex position by computing neighbouring old vertices

### Subdivision matrix
Subdivision matrix maps k-ring neighbour to next level. Assume n vertices for k-ring neighbour, $\bm{S} \in \mathbb{R}^{(n+1) \times (n+1)}$, as it also needs to update old vertex.

#### Eigen analysis
In order to keep multiply subdivision matrix, it's easier to apply eigen analysis, convert basis to [[eigenbasis|linear-algebra.basis#eigenbasis]], apply all transformation and convert back.

- Converge to tangent: single largest eigenvalue of 1 with corresponding eigenvector $\bm{1}$

> TODO: why is it necessary?

## TODO
- [x] Dyadic split
- [x] Smoothness $C^1$ $C^2$
- [x] Extraordinary vertices
- [ ] Stencil
- [x] Permutation matrix
- [x] Eigen structure
- [ ] Converge