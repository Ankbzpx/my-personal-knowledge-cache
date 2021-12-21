---
id: RW7qEKrWU3lCON1AOppT3
title: Rotate Landmarks in Normalized Coordinate
desc: ''
updated: 1640018715774
created: 1640018707366
---

```
Image coordinate                  Normalized coordinate
0------------+------ img_w        0------------------+-----------1
|            |           |        |                  |           |
|            |           |        |                  |           |
|            |           |        |                  |           |
|            |           |        |                  |           |
|           y|           |        |               y_n|           |
|       +-------------+  |        |         +-----------------+  |
|       |     ^       |  | ======>|         |        ^        |  |
|   x   |    h|       |  |        |   x_n   |     h_n|        |  |
+------>|     v       |  |        +------>  |        v        |  |
|       +-------------+  |        |         +-----------------+  |
|       <------------->  |        |         <----------------->  |
|            w           |        |                 w_n          |
img_w -------------------+        1------------------------------+
```

- Move coordinate from top left to center
- Scale coordinate such that x, y axis units are equally scaled `x = x / img_w * img_h`
- Apply 2d rotation matrix $\begin{bmatrix}\cos(\theta) & \sin(\theta)\\-\sin(\theta) & \cos(\theta)\end{bmatrix}$
- Scale coordinate back `x = x / img_h * img_w`
- Move coordinate back from center to top left