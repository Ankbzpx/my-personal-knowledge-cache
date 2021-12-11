---
tags: [Rendering]
title: Shading
created: '2021-12-11T08:39:11.678Z'
modified: '2021-12-11T08:48:20.649Z'
---

# Shading

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
