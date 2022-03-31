---
id: DziT9w00zlJ6lNNuObTyb
title: Storage
desc: ''
updated: 1640018104279
created: 1640018060695
---

Usually (like in openGL) contiguous in memory: 

$$
\begin{bmatrix} x_{0} & x_{1} & x_{2} & 0 & y_{0} & y_{1} & y_{2} & 0 & z_{0} & z_{1} & z_{2} & 0 & w_{0} & w_{1} & w_{2} & 1 \end{bmatrix}
$$

In practice, the multiplication is equivalent to 
$$
v_0\begin{bmatrix} x_0 & x_1 & x_2 \end{bmatrix} + v_1\begin{bmatrix} y_0 & y_1 & y_2 \end{bmatrix} + v_2\begin{bmatrix} z_0 & z_1 & z_2 \end{bmatrix}
$$
