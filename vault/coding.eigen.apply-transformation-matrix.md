---
id: y103fyh4zyjiv0olsk17ite
title: Apply Transformation Matrix
desc: ''
updated: 1646984766200
created: 1646984729555
---

```
V: n x 3
TT: 4 x 4 transformation matrix

(V.rowwise().homogeneous()*TT).rowwise().hnormalized())
```
