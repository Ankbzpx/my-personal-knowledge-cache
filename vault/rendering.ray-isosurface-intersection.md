---
id: 5j1fqde4laozjc211gc8zwk
title: Ray Isosurface Intersection
desc: ''
updated: 1657955059564
created: 1657953624846
---
> Reference: https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.118.8397&rep=rep1&type=pdf

- Traverse cells in volume checking bounds for isovalue, can be solved in close form as root of cubic polynomial
- Speed up traversal using spatial index (like R-tree)