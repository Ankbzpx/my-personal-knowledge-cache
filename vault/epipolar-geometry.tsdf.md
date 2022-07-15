---
id: r8y2bovxrlx6vkhn7my8e0m
title: TSDF
desc: ''
updated: 1657877880179
created: 1657877868745
---

> Reference: https://graphics.stanford.edu/papers/volrange/volrange.pdf

- Continuous implicit function $D(\bm{x})$, denotes the weighted signed distance from $\bm{x}$ to the nearest surface along viewing ray. Note that it is not but an approximation to the signed distance to nearest surface.
- Cumulative signed distance function $D(\bm{x})$ and cumulative weight $W(\bm{x})$ are formed by combining $d(\bm{x})$ and $w(\bm{x})$ for each depth images.
- $D(\bm{x})$ is evaluted at discrete voxel grid to extract iso surface
- Weight incorporates uncertainty in measurement (i.e. cosine of angle between vertex normal and viewing angle)
- Weight needs to fall off at the max uncertainty interval in front of / behind the surface (restrict at the vicinity of the surface)

Combine rule:
$$
D(\bm{x}) = \frac{\sum w_i(\bm{x}) d_i(\bm{x})}{\sum w_i(\bm{x})}
$$
$$
W(\bm{x}) = \sum w_i (\bm{x})
$$

Incremental calculation (combine depth image one by one):
$$
D_{i+1}(\bm{x}) = \frac{W_i(\bm{x})D_i(\bm{x}) + w_{i+1}(\bm{x})d_{i+1}(\bm{x})}{W_i(\bm{x}) + w_{i+1}(\bm{x})}
$$
$$
W_{i+1}(\bm{x}) = W_i(\bm{x}) + w_{i+1}(\bm{x})
$$