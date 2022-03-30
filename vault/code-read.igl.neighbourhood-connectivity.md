---
id: ww8z42lj1f2cm9lj0yx3ubc
title: Neighbourhood Connectivity
desc: ''
updated: 1647584184335
created: 1647231617065
---

## Concepts
### Manifold mesh
> Reference: https://pages.mtu.edu/~shene/COURSES/cs3621/SLIDES/Mesh.pdf

- Each edge is incident to only one or two faces
- Faces incident to a vertex form a closed or an open fan

## vertex_triangle adjacency

### vertex_triangle_adjacency

#### Vector of vector

Loop through all faces with their vertex indices, push back occurrence

|Vert index|Incident Face indices|
|:-------- |:------------------- |
|0         | 0, 1                |
|1         | 9, 13, 1            |

#### Unrolled

```
vfd(i): number of incident faces at vertex i
NI: cumsum of vfd, prepend 0
NI(i): number of cummulated incident faces before vertex i
VF(NI(i) + k): kth face incident on vertex i (vertex index -> face index)
```

We unroll and store it into array and index it by cumsum

Array:
```
[0, 1, 9, 13, 1]
```

Index:
```
[0, 2, 5]
```

## vertex_vertex adjacency
### adjacency_list

1. Loop through all faces with their vertex indices, for the vector of each vertex index, push back all other vertex indices
2. sort and remove duplicates


> My numpy implementation: [[Implementation|polygon.connect-polygons-with-their-offset-ones#implementation]]

## edge_edge adjacency

### facet_components

Compute number of connected facets and assign each face an id based on its connected components

1. Compute face adjacency matrix using [[facet_adjacency_matrix|code-read.igl.neighbourhood-connectivity#facet_adjacency_matrix]]
2. Perform **breadth first search** on adjacency matrix row-wise, increase id when each row finished

### facet_adjacency_matrix

1. Call [[unique_edge_map|code-read.igl.neighbourhood-connectivity#unique_edge_map]], we get a list of oriented edges indices that share the same unique unoriented occurence
2. From [[oriented_facets|code-read.igl.neighbourhood-connectivity#oriented_facets]], by modding oriented facets index over NF, we can get its face index
3. Combine 1 and 2, we know which faces share the same edge, thus a NF x NF sparse adjacency matrix can be constructed

### unique_edge_map

1. Find all oriented edges using [[oriented_facets|code-read.igl.neighbourhood-connectivity#oriented_facets]] then filter using [[unique_simplices|code-read.igl.neighbourhood-connectivity#unique_simplices]]. (faster than directly finding unoriented edges) (`EMAP` is `IC` in [[unique_rows|code-read.igl.neighbourhood-connectivity#unique_rows]], used to construct all oriented edges from unique unoriented edges, 3NF)

2. Same idea as [[Unrolled|code-read.igl.neighbourhood-connectivity#unrolled]], unroll a list of oriented edges indice (3NF) grouped by unique unoriented occurance, as well as a array of index, with each element be the number of oriented edges before the first of unique unoriented occurrence, NUE(number of unique unoriented edges) + 1

|Unique edge index|edge indice in edge list|
|:-------- |:------------------- |
|0         | 0, 10               |
|1         | 9, 13, 1            |

```
VectorXI uEK;
igl::accumarray(EMAP,1,uEK);
```
computes occurance for each unique edges

```
uEC: cumsum of uEK as index
uEO: array used for counting
e: the e'th edge in oriented edge lists
ue = EMAP(e): index of unique unoriented edge corresponding to e'th edge in oriented edge lists
uEE(uEC(ue)+ uEO(ue)) = e: record oriented edge index
```

### oriented_facets

For manifold triangle mesh

Given $F \in \mathbb{Z}^{n \times 3}$, so that

$$
E = 
\begin{bmatrix}
F[:, 1] & F[:, 2] \\
F[:, 2] & F[:, 0] \\
F[:, 0] & F[:, 1] \\
\end{bmatrix}
\in 
\mathbb{Z}^{3n \times 2}
$$

### unique_simplices

Given N x D simplices matrix (D is the dimension of simplex)

1. Sort row-wise
2. Keep unique ones by calling [[unique_rows|code-read.igl.neighbourhood-connectivity#unique_rows]]

### unique_rows

```
A: input
C: unique rows
IA: index to construct C from A
IC: index to construct A from C
```

Similary to `np.unique`, see [[snippets.remove_duplicate_vertices]]
- `IA` -> `unique_indices`
- `IC` -> `unique_indices`


## TODO
- [x] Manifold mesh