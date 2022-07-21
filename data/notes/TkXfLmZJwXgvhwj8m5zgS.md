
## Model Matrix
Object w.r.t. World (O -> W)

$$
T_W = T_{WO} T_{O}
$$

**IMPORTANT** ([[linear-algebra.matrix-matrix-dot-product]])

- A w.r.t. B means A's basis in B's coordinate system
- $T_O$ transforms coordinate in $O$ to standard basis $S$. Columns of $T_O$ are basis of $O$ seen in $S$. It can also be written as $T_{SO}$, $O$ w.r.t. $S$, or just a linear transformation in $S$

## View Matrix
> **IMPORTANT** Inverse Camera View Matrix is its Model Matrix
 
World w.r.t. Camera (W -> C)

$$
T_C = T_{CW} T_{W}
$$

## Projection Matrix
> **IMPORTANT** OpenGL(rendering) camera is different from epipolar geometry camera in computer vision, More see: https://amytabb.com/tips/tutorials/2019/06/28/OpenCV-to-OpenGL-tutorial-essentials/

Camera w.r.t. Normalized Device coordinate (C -> NDC)

$$
T_{NDC} = T_{NDC,C} T_{C}
$$
