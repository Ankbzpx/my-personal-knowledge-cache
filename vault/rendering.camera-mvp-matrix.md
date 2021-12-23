---
id: TkXfLmZJwXgvhwj8m5zgS
title: Camera MVP Matrix
desc: ''
updated: 1640158698744
created: 1640157654319
---

## Model Matrix
Object w.r.t. World (O -> W)

$$
T_W = T_{WO} T_{O}
$$

## View Matrix
World w.r.t. Camera (W -> C)

$$
T_C = T_{CW} T_{W}
$$

## Projection Matrix
Camera w.r.t. Image (C -> I)

$$
T_I = T_{IC} T_{C}
$$
