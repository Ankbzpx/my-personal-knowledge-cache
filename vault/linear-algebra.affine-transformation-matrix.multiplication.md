---
id: fF2znCo5IQbZtOIIep5OD
title: Multiplication
desc: ''
updated: 1641299696131
created: 1640017982642
---
## Row Major
$$
\bm{v} \cdot \bm{A} = \begin{bmatrix} v_0 & v_1 & v_2 \end{bmatrix} \left[\begin{array}{ccc} x_0 & x_1 & x_2 \\ \hline y_0 & y_1 & y_2 \\ \hline z_0 & z_1 & z_2 \end{array}\right] = \begin{bmatrix} x_0 v_0 + y_0 v_1 + z_0 v_2 & x_1 v_0 + y_1 v_1 + z_1 v_2 & x_2 v_0 + y_2 v_1 + z_2 v_2 \end{bmatrix}
$$

## Column Major
$$
\bm{A} \cdot \bm{v} = \left[\begin{array}{c|c|c} x_0 & y_0 & z_0 \\ x_1 & y_1 & z_1 \\ x_2 & y_2 & z_2 \end{array}\right] \begin{bmatrix} v_0 \\ v_1 \\ v_2 \end{bmatrix} = \begin{bmatrix} x_0 v_0 + y_0 v_1 + z_0 v_2 \\ x_1 v_0 + y_1 v_1 + z_1 v_2 \\ x_2 v_0 + y_2 v_1 + z_2 v_2 \end{bmatrix}
$$