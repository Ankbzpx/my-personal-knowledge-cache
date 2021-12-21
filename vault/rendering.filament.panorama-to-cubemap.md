---
id: G2j5Uy9n4yetQ4YYe1wgM
title: Panorama to Cubemap
desc: ''
updated: 1640019358289
created: 1640019300606
---

> Reference: https://stackoverflow.com/questions/29678510/convert-21-equirectangular-panorama-to-cube-map

## Core ideas:
- View panorama as the equirectangular projection of sphere, such that spherical coordinate can be directly used to query its pixel value
- Map from target to source (inverse transformation), by finding the spherical representation of cartesian coordinate in target image
- Perform sampling and interpolation

## Details 
### Spherical coordinates to Cartesian coordinate convertion
$$
{\begin{cases} x= r\sin\theta\cos\phi \\ y = r\sin\theta\sin\phi \\ z=r\cos\theta \end{cases}}
$$
Where
$$
\begin{cases} r=1 \\ 0<\theta<\pi \\ -\frac{\pi}{4} < \phi < \frac{7\pi}{4} \end{cases}
$$
### Project to sides
Let $x = 1$ such that $a = \frac{1}{\sin\theta\cos\phi}$
So
$$
y = \tan\phi, z = \frac{1}{\tan\theta\cos\phi}
$$
Then
$$
\phi = atan2(y), \theta = atan2(\frac{1}{z\cos\phi})
$$

### Project to top
Let $z = 1$ such that $a = \frac{1}{\cos\theta}$
So
$$
x = \tan\theta\cos\phi, y = \tan\theta\sin\phi
$$
Then
$$
\phi = atan2(\frac{y}{x}), \theta = atan2(\sqrt{x^{2} + y^{2}})
$$

## Python sample code
```
# left, front, right, back, top, bottom
offset = [0, 0.25, 0.5, 0.75, 0, 0]

face_id = 0

for w in range(edge):
    for h in range(edge):
        
        # map to range (-1, 1)
        u = (w/edge - 0.5)*2
        v = (h/edge - 0.5)*2
        
        if face_id == 4:
            phi = pi - atan2(v, u)
            theta = atan2(sqrt(v*v+u*u), 1)
        elif face_id == 5:
            phi = -pi + atan2(v, u)
            theta = -atan2(sqrt(v*v+u*u), 1)
        else:
            phi = atan2(u, 1)
            theta = atan2(-1, v*cos(phi))
        
        x = phi/(2*pi)
        y = theta/pi
        
        # TODO: use bilinear interpolation
        x_idx = int(width*(x + offset[face_id]))
        y_idx = int(height*y)
        
        face_pixel[w, h] = panorama_pixel[x_idx, y_idx]
```