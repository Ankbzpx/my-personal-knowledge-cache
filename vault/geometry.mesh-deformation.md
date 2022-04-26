---
id: 32xk45x8lisds1ti4bx4h4y
title: Mesh Deformation
desc: ''
updated: 1650940693333
created: 1650869560564
---

> Reference: https://lgg.epfl.ch/publications/2006/botsch_2006_DTD.pdf

## Variational minimization approach
Suface $\mathcal{S}$ = low-frequency based surface $\mathcal{B}$ + high-frequency geometry details $\mathcal{D}$

### Typical steps
1. Compute $\mathcal{B}$ from $\mathcal{S}$
2. Encode details $\mathcal{D}$ as displacement w.r.t. $\mathcal{B}$
3. Deform $\mathcal{B} \to \mathcal{B}'$
4. Apply the details $\mathcal{S}' =\mathcal{B}' + \mathcal{D}$


### Limitations
1. Difference between $\mathcal{S}$ and $\mathcal{B}$ needs to be small enough that the a height field w.r.t. $\mathcal{B}$ is sufficient to represent it (otherwise more hierarchies are needed)
2. Displacement may cause self-intersection

## Deformation Transfer

### Definition
For a source mesh in original state $\mathcal{S}$ (with vertex $\bm{q}_i$) and deformed state $\mathcal{S}'$ (with vertex $\bm{q}_i'$), a source deformation is computed (with gradient $\bm{S}_j$) for each triangle. 

The per-triangle deformation $\bm{S}_j$ can be calculated as 
$$
\bm{S}_j = (\bm{q}_1' - \bm{q}_3', \bm{q}_2' - \bm{q}_3', \bm{n}') \cdot (\bm{q}_1 - \bm{q}_3, \bm{q}_2 - \bm{q}_3, \bm{n})^{-1}
$$
where $\bm{q}_i$ $\bm{q}_i'$ and $\bm{n}$ $\bm{n}'$ are triangle vertex position and normal in $\mathcal{S}$ or $\mathcal{S}'$.

The goal is to find new deformed vertex position for a target mesh, such that the target deformation gradient matches the source ones $\bm{S}_j$.

The new vertex position $\bm{p}_i'$ can be calculated as
$$
\bm{G}^T \bm{D} \bm{G}
\begin{bmatrix}
{\bm{p}_1'}^T \\
\vdots \\
{\bm{p}_n'}^T
\end{bmatrix}
=
\bm{G}^T \bm{D}
\begin{bmatrix}
{\bm{S}_1'}^T \\
\vdots \\
{\bm{S}_m'}^T
\end{bmatrix}
$$

where $\bm{G} \in \mathbb{R}^{3m \times n}$ as gradient operator (More see [[grad|code-read.igl.mesh-parameterization#grad]]), $\bm{D} \in \mathbb{R}^{3m \times 3m}$ is diagonal triangle-based weight matrix, $\bm{G}^T \bm{D}$ is divergence operator, $\bm{G}^T \bm{D} \bm{G}$ is divergence of gradient, a.k.a. discrete laplacian operator.

> **IMPORTANT** Above equation only constraints gradient. Translational contraints are also needed when performing mesh deformation

### Application in mesh deformation

1. Compute $\mathcal{B}$ from $\mathcal{S}$
2. Deform $\mathcal{B} \to \mathcal{B}'$ and compute per-triangle deformation gradient $\bm{S}_j$
3. Transfer $\mathcal{S} \to \mathcal{S}'$ by complying $\bm{S}_j$

## TODO:
- [ ] Thin shells energy
- [ ] Gradient based deformation (suffers from translation insensitivity)
- [ ] Discrete divergence operator