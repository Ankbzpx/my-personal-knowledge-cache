---
id: rofvgoy9bqzswabd2hlhmj0
title: Subdivision
desc: ''
updated: 1647587102748
created: 1647574129027
---

More info: [[survey.subdivision]]

## Concepts
### Barycenter
Center of mass

### Edge-manifold
Every edge are either boundary or incidents two faces (oppositely oriented)

## Implementations

### barycenter

Average of all [[Simplex|survey.winding-number#simplex]] vertex coords

### edge_topology

1. Verify [[is_edge_manifold|code-read.igl.subdivision#is_edge_manifold]]
2. Collect [smaller_vert_idx, larger_vert_idx, face_idx, edge_idx_in_face] for all edges, then sort it [[lexicographically|coding.cpp.container#vector-comparison]]
3. After sorting, each two consecutive edge are equal (boundary) or not (incident two faces), keep track of edge_vert (NE x 2), edge_face (NE x 2), face_edge (NF x 3) relations
4. For edge_face relations, make sure first face on the left of edge, by checking if this oriented edge appears on the first face


### is_edge_manifold

1. Compute [[unique_edge_map|code-read.igl.neighbourhood-connectivity#unique_edge_map]]
2. Count occurence of each unique unoriented edges, check if all are less or euqal to 2
