---
id: 4Fdxoie9kYc9C9NvJuieK
title: Shading
desc: ''
updated: 1647225512644
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

## Data types
- `const`: compile time constant
- `uniform` (per-primitive): constant during entire draw call

### Before 4.2
- `attribute` (per-vertex): position, normal, uv, etc.
- `varying` (per-fragment): Interpolated between vertex and fragment shader

### 4.2+

> Both `attribute` and `varying` are deprecated

- `in`: linkage in from previous stage
- `out`: linkage out to next stage

> Reference: https://gamedev.stackexchange.com/questions/29672/in-out-keywords-in-glsl


## Pass data from Vertex to Fragment shader

### Before 4.2
Declare `varying` variable in both shaders


### 4.2+
Declare `out` for vertex shader, `in` for fragement shader

## Render order
- Opaque: front to back
- Transparent: back to front

> Portal effect: Render occlusion object first (write depth but no color), then render the rest. Fragment further than occlusion depth will be discarded.