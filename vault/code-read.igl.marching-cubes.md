---
id: c88qr3qkmys9clilexzpudj
title: Marching Cubes
desc: ''
updated: 1648528334896
created: 1648528334896
---
Construct vertices and faces from grid sample points. For each cube:
1. Determine cube type by comparing each cube's 8 corner values with iso-surface value
2. Query its vertex and face config from predetermined $2^8$-entry table

## Implementation

### Data structure
- `EdgeKey`: 2 indice
- `EdgeHash`: Hash function for `EdgeKey`
- `edge2vertex`: std::unordered_map<EdgeKey, unsigned int>
- `offset`: flattned index of following coordinates
$$
\begin{bmatrix}
0 & 0 & 0 \\
1 & 0 & 0 \\
1 & 1 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 \\
1 & 0 & 1 \\
1 & 1 & 1 \\
0 & 1 & 1 \\
\end{bmatrix}
$$

### Constructor
1. For each cube, obtain 8 corner flattened index using offset

2. For each corner, query its value using flattened index, compare it with iso-surface value, determine its `cubetype` (char, 1 byte) by leftshift `1` n bit and apply logical OR

3. Reject cubes with all positve/negative corner values

4. Query `cubetype` in `edgeTable` to check which edge needs adding new vertex. Apply [[add_vertex|code-read.igl.marching-cubes#add_vertex]] if type matches

5. Query `cubetype` in `triTable` to connect and add new faces

### add_vertex
Compute new vertex on given cube edge by linear interpolating cube edge corner values 


## TODO
- [ ] edgeTable
- [ ] triTable

