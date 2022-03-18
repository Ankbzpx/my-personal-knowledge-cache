---
id: xi244lw2v12vzkedvax2sx6
title: Matcap
desc: ''
updated: 1647583824043
created: 1646987186972
---

Material Capture

> Reference: https://github.com/nidorx/matcaps

Complete material from image without actual compute reflection and lighting

## Implementation

### Normal matrix

View matrix: W -> Camera

See: [[rendering.camera-mvp-matrix]] and [[linear-algebra.affine-transformation-matrix.multiplication.transform-normal-vector]]

```
norm = view.inverse().transpose();
```

### Vertex shader

```
normal_eye = vec3 (normal_matrix * vec4 (normal, 0.0));
normal_eye = normalize(normal_eye);
```

### Fragment shader

```
vec2 uv = normalize(normal_eye).xy * 0.5 + 0.5;
outColor = texture(tex, uv);
```
