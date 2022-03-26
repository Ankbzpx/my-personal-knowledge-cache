---
id: TkXfLmZJwXgvhwj8m5zgS
title: Camera MVP Matrix
desc: ''
updated: 1648040869444
created: 1640157654319
---

## Model Matrix
Object w.r.t. World (O -> W)

$$
T_W = T_{WO} T_{O}
$$

**IMPORTANT** ([[Matrix matrix dot product|linear-algebra.basis#matrix-matrix-dot-product]])

- A w.r.t. B means A's basis in B's coordinate system
- $T_O$ transforms coordinate in $O$ to standard basis $S$. Columns of $T_O$ are basis of $O$ seen in $S$. It can also be written as $T_{SO}$, $O$ w.r.t. $S$, or just a linear transformation in $S$

## View Matrix
> Inverse Camera View Matrix is its Model Matrix
 
World w.r.t. Camera (W -> C)

$$
T_C = T_{CW} T_{W}
$$

## Projection Matrix
Camera w.r.t. Image (C -> I)

$$
T_I = T_{IC} T_{C}
$$
