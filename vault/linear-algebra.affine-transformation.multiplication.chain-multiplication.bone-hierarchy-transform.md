---
id: C7fAfJkVy70R80Cw7ecYN
title: Bone Hierarchy Transform
desc: ''
updated: 1640018656669
created: 1640018577910
---

Given Bone A, B, C, D with their corresponding local tranformation matrix ${T_{A}}$, $T_B = {T_{AB}}$, $T_C = {T_{BC}}$, $T_D = {T_{CD}}$
```
parent -> child
A -> B -> C -> D
```
Their world transformation matrix can be represented by

$$
{T_{WA} = T_A}
$$

$$
{T_{WB} = T_A * T_B}
$$

$$
{T_{WC} = T_A * T_B * T_C}
$$

$$
{T_{WD} = T_A * T_B * T_C * T_D}
$$

If we only want to modify the local transformation of B to B' by

$$
{T_{B'} = T_{B} * T_{BB'}}
$$

It causes chain reaction that B's child bones' world transformation no long hold

$$
{T_{WC} \neq T_A * T_{B'} * T_C}
$$

$$
{T_{WD} \neq T_A * T_{B'} * T_C * T_D}
$$

It can be fixed by

$$
{T_{C} \rightarrow T_{C'} = T_{B'B} * T_C}
$$

$$
{T_{D} \rightarrow T_{D'} = T_{C'C} * T_D}
$$
