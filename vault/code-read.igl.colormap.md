---
id: 97gnlpow8yszfzt0g77kc9c
title: Colormap
desc: ''
updated: 1647314198349
created: 1647230672465
---

Maps value given color palette

## Implementation
> colormap

```
palette p: 256 x 3 (RGB8) array
factor f: [0, 1]

mapped_f = f * (256 - 1)
a = floor(f)
b = ceil(f)

# apply linear interpolation for rgb
c = mix(p[a], p[b], mapped_f - a)
```

## Fragment shader

Substitude material diffuse [[Phong lighting model|rendering.rendering-terms#phong-lighting-model]]

> Bind buffer

```
bind_vertex_attrib_array(shader_mesh,"Kd", vbo_V_diffuse, V_diffuse_vbo, dirty & MeshGL::DIRTY_DIFFUSE);
```
> Vertex shader (for interpolation)

```
Kdi = Kd;
```

> Fragment shader

```
vec4 color = vec4(lighting_factor * (Is + Id) + Ia + (1.0-lighting_factor) * vec3(Kdi),(Kai.a+Ksi.a+Kdi.a)/3);
```