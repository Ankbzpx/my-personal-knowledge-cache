---
id: 4Fdxoie9kYc9C9NvJuieK
title: Shading
desc: ''
updated: 1641881430756
created: 1640019527539
---

## Vertex & Fragment shader
```
Vertex Shader -> every vertex
 |
 | Coordinate interpolation
 v
Fragment Shader -> every pixel
```
### Pass data from Vertex to Fragment shader
> OpenGL interpolate `varying` variable

Declare `varying` variable in both shaders


## Render order
- Opaque: front to back
- Transparent: back to front

Portal effect: Render occlusion object first (write depth but no color), then render the rest. Fragment further than occlusion depth will be discarded.