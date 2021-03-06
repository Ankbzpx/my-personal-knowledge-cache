---
id: 165c63izf6p5re979g2aht3
title: Mesh Smoothing
desc: ''
updated: 1649382213948
created: 1649382213948
---

## Concepts

### Bilateral filter
> Reference: https://en.wikipedia.org/wiki/Bilateral_filter

Edge-perserving + noise-reducing 

$$
I^{filtered} (x) = \frac{1}{W_p} \sum_{x_i \in \Omega} I(x_i) f_r(\|I(x_i)-I(x)\|)g_s(\|x_i - x\|)
$$
where
$$
W_p = \sum_{x_i \in \Omega} f_r(\|I(x_i)-I(x)\|)g_s(\|x_i - x\|)
$$
$\Omega$ is the window centered at $x$, $f_r$ is intensity range kernel, $g_s$ is spatial distance kernel

> Larger the intensity difference, smaller the weight it has on bluring. (Blur pixel with similar intensity while perserves sharp intensity changes)

## Explicit laplacian smoothing
> Reference: http://multires.caltech.edu/pubs/ImplicitFairing.pdf

Move vertex along the opposite of its 1-ring neighbour normal $\bm{n}$

$$
\bm{v}_{i+1} = (I - \lambda L) \bm{v}_i
$$


### Uniform weighted
> Reference: https://graphics.stanford.edu/courses/cs468-12-spring/LectureSlides/06_smoothing.pdf

$$
L_V(\bm{v}) = (\frac{1}{n} \sum_{} \bm{v}_i) - \bm{v}
$$

### Cotangent weighted

The movement is proportioonal to the curvature. See [[Laplace-Beltrami|geometry.discrete-laplace-operator#laplace-beltrami]]

$$
L(\bm{v}) = 2\kappa_H \bm{n}_i
$$

## Bilateral denoising
> Reference: https://www.cs.tau.ac.il/~dcor/articles/2003/Bilateral-Mesh-Denoising.pdf

Generalization of Bilateral filter for mesh, with distance to tangent plane as intensity difference.

### Notes

#### Denoising mesh over point cloud
- connetivity
- fast neighbourhood access

#### Mesh info decomposition
- tangential: parametric info
- nomral: geometric info

> Move vertex along the direction of normal only modify the geometry of the mesh

#### Comparison with image processing
-  Irregular in both connectivity and sampling
- Non-enegry perserving leads to mesh shrinkage
- Vertex drift, mesh regularity decreases

## TODO
- [ ] [Implicit Laplacian smoothing](http://mesh.brown.edu/taubin/pdfs/taubin-sg95.pdf)
- [ ] [Fourier transform on graph as linear combination of eigenvectors (frequencies) of laplacian operator](https://www.math.ucla.edu/~tao/preprints/fourier.pdf)
- [ ] Mean curvature as divergence of normal vector $\kappa_H = div \bm{n}$
- [ ] Vertex Drifting and mesh regularity
- [x] Bilateral filter
- [ ] Bilateral filter the Bayesian approach