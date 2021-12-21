---
id: 4Fdxoie9kYc9C9NvJuieK
title: Shading
desc: ''
updated: 1640054556179
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

## Render order
- Opaque: front to back
- Transparent: back to front

Portal effect: Render occlusion object first (write depth but no color), then render the rest. Fragment further than occlusion depth will be discarded.