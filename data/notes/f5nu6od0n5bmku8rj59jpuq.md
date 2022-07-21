
Perspective 3 points (P3p) is used for pose estimation given 2D-3D correspondence

## Points fitting approach

$$
\bm{x} = \bm{K} [\bm{R} | \bm{t}] \bm{X}
$$

Direct fit by pairing [[optimization.least-square]] with [[epipolar-geometry.ransac]], suboptimal

## Algebraic approach

![](/assets/images/2022-05-14-08-00-55.png){width: 350px}

$$
\begin{cases}
s_1^2 = l_A^2 + l_B^2 - 2 l_A l_B \cos\theta_{AB} \\
s_2^2 = l_A^2 + l_C^2 - 2 l_A l_C \cos \theta_{AC} \\
s_3^2 = l_B^2 + l_C^2 - 2 l_B l_C \cos \theta_{BC}
\end{cases}
$$

4 positive solution for 3 points, use 4th point to determine correct solution

## Other approaches
- AP3p: directly solve camera pose
- EPnp: for $n \geq 4$, $n$ points as weighted sum of 4 control points