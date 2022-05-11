---
id: xecmhi0elpdg8uuoed7hzmo
title: Pinhole Camera Model
desc: ''
updated: 1652245195551
created: 1652239328730
---

## Base case

> Reference: http://www.songho.ca/opengl/gl_projectionmatrix.html

Begin with the assumption that 
- Camera with its center $c$ placed at orgin and focal length in pixel $f$
- Image plane coordinate origin is the principal point
- World coordinate and Image coordinate are evenly scaled along all axis

> Under square pixel assumption, $\text{focal length in pixel} = \text{camera frame width} * \text{focal length in mm} / \text{sensor width in mm}$

For a point $\bm{X} = (X, Y, Z)^T$ and it projection on image plane $\bm{x} = (x, y)^T$

![](/assets/images/2022-05-11-11-22-39.png){width: 500px}

As is shown in figure, we have
$$
\frac{f}{Z} = \frac{x}{X} = \frac{y}{Y}
$$

$$
\therefore
\begin{cases}
x = \frac{fX}{Z} \\
y = \frac{fY}{Z}
\end{cases}
$$

The projection matrix $\bm{P}$ is the one that transforms $\bm{X} = (X, Y, Z)^T$ to $\bm{x} = (x, y)^T = (\frac{fX}{Z}, \frac{fY}{Z})^T$

$$
\begin{bmatrix}
x \\ y \\ 1
\end{bmatrix}
=
\begin{bmatrix}
\frac{fX}{Z} \\ \frac{fY}{Z} \\ 1
\end{bmatrix}
=
\begin{bmatrix}
fX \\ fY \\ Z
\end{bmatrix}
=
\begin{bmatrix}
f & 0 & 0 & 0 \\ 
0 & f & 0 & 0 \\ 
0 & 0 & 1 & 0
\end{bmatrix}
\begin{bmatrix}
X \\ Y \\ Z \\ 1
\end{bmatrix}
$$

## Image plane coordinate not at principal center

Assume principal point offset w.r.t. image plane coordinate is $(p_x, p_y)$

![Image plane](/assets/images/2022-05-11-11-24-00.png){width: 300px}

Similar as before, but with $\bm{x} = (x, y)^T = (\frac{fX}{Z} + p_x, \frac{fY}{Z} + p_y)^T$

$$
\begin{bmatrix}
x \\ y \\ 1
\end{bmatrix}
=
\begin{bmatrix}
\frac{fX}{Z} + p_x \\ \frac{fY}{Z} + p_y \\ 1
\end{bmatrix}
=
\begin{bmatrix}
fX + Zp_x \\ fY + Zp_y \\ Z
\end{bmatrix}
=
\begin{bmatrix}
f & 0 & p_x & 0 \\ 
0 & f & p_y & 0 \\ 
0 & 0 & 1 & 0
\end{bmatrix}
\begin{bmatrix}
X \\ Y \\ Z \\ 1
\end{bmatrix}
$$

## Camera center not at the origin of world coordinate

Assume the world origin transformation w.r.t. camera center is $\begin{bmatrix}
\bm{R} & \bm{t} \\ \bm{0} & 1 \end{bmatrix}$

![Camera in world coordinate](/assets/images/2022-05-11-11-24-54.png){width: 150px}

We can then transform $\bm{X}$ from world coordinate system to camera coordinate system (with camera center at origin) then apply the projection
$$
\bm{x} = \bm{K}[\bm{R}|\bm{t}]\bm{X}
$$

$\bm{K} = \begin{bmatrix}
f & 0 & p_x \\ 
0 & f & p_y \\ 
0 & 0 & 1
\end{bmatrix}$ in camera intrinsic matrix, $[\bm{R}|\bm{t}] \in \mathbb{R}^{3 \times 4}$ is top 3 rows of the [[View Matrix|rendering.camera-mvp-matrix#view-matrix]]

> **IMPORTANT** [[View Matrix|rendering.camera-mvp-matrix#view-matrix]] is the inverse of its [[Model Matrix|rendering.camera-mvp-matrix#model-matrix]], a.k.a. the camera's transform in world coordinate system

## Non-square pixel
If the CMOS has non-square pixel, the image plane coordinate are not even scaled along $x$, $y$ axis.

Denote focal lengths (in pixel) in $x$, $y$ directions are $f_x$, $f_y$

$\bm{K} = \begin{bmatrix}
f_x & 0 & p_x \\ 
0 & f_y & p_y \\ 
0 & 0 & 1
\end{bmatrix}$

### Relation to field-of-view

![field of view](/assets/images/2022-05-11-11-25-13.png){width: 150px}

As is illustrated in the figure

$$
fov_x = 2 \arctan (\frac{0.5 * w}{f_x})
$$
$$
fov_y = 2 \arctan (\frac{0.5 * h}{f_y})
$$
where $w$, $h$ are image width, height

## Change of image coordinate

> Reference: https://dsp.stackexchange.com/questions/6055/how-does-resizing-an-image-affect-the-intrinsic-camera-matrix

Assume original $\bm{K} = \begin{bmatrix}
f_x & 0 & p_x \\ 
0 & f_y & p_y \\ 
0 & 0 & 1
\end{bmatrix}$

Scaling example, $(x, y)^T \to (ax, by)^T$

![Change of image coordinate](/assets/images/2022-05-11-11-25-32.png){width: 250px}

Let scaling matrix
$\bm{S} = 
\begin{bmatrix}
a & 0 &0 \\
0 & b & 0 \\
0 & 0 & 1 
\end{bmatrix}$

$\bm{K'} = \bm{S} \bm{K} = \begin{bmatrix}
f_x / a & 0 & p_x / a \\ 
0 & f_y / b & p_y / b \\ 
0 & 0 & 1
\end{bmatrix}$

In general, find $3 \times 3$ transformation matrix that transforms the image coordinate, then right multiply it with $\bm{K}$, a.k.a. apply the transformation
