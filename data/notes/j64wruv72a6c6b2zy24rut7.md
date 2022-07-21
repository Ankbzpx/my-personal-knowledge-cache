
## Surface Measurement
### Notations
- $\bm{T}_{g,k}$: camera pose at time $k$ ($\bm{T}_{g,k}$ is camera [[Model Matrix|rendering.camera-mvp-matrix#model-matrix]], $\bm{T}_{g,k}^{-1}$ is camera [[View Matrix|rendering.camera-mvp-matrix#view-matrix]])
- $K$: camera intrinsic matrix
- $\bm{p}_k$: point in camera frame at time $k$
- $\bm{p}_g$: point in world frame
- $\bm{q} = \pi(\bm{p})$: point in image frame (hnormalized)
- $R_k$: Raw depth map at time $k$
- $\bm{u} = (u, v)$: image pixel
- $\dot{\bm{u}} = [\bm{u}^T|1]^T$: image pixel in homogenous coordinate
- $R_k(\bm{u})$: depth value at pixel $\bm{u}$

### Known relations
$$
\bm{p}_g = \bm{T}_{g,k} \bm{p}_k
$$


$$
\bm{p}_k = R_k(\bm{u})\bm{K}^{-1}\dot{\bm{u}}
$$

### Steps
1. Apply [[Bilateral filter|geometry.mesh-smoothing#bilateral-filter]] to obtain denoised depth map $D_k$
2. Back-project filtered depth to obtain vertex map $\bm{V}_k(\bm{u}) = D_k(\bm{u})\bm{K}^{-1}\dot{\bm{u}}$
3. Compute normal map $\bm{N}_k(\bm{u}) = normalized((\bm{V}(u+1, v) - \bm{V}(u, v))\times(\bm{V}(u, v+1)- \bm{V}(u, v)))$
4. Compute binary mask map $\bm{M}_k$ for valid vertices
5. Computer 3 level vertex normal map pyramid via mipmapping $D_k$

## Mapping as Surface Reconstruction

### Notations
- $F_k(\bm{p}_g)$: truncated signed distance value
- $W_k(\bm{p}_g)$: weight
- $\bm{S}_k(\bm{p}_g)$: [[epipolar-geometry.tsdf]]
- $\mu$: measure uncertainty
- $r$: distance from camera center along depth ray
- $\lambda = \| \bm{K}^{-1} \dot{\bm{u}} \|_2$: distance along depth ray (distance between projected $\bm{u}$ and camera center)

> **IMPORTANT** Unproject image point is depth ambiguous

### TSDF representation
1. Region within uncertainty measurement: $|r - \lambda R_k(\bm{u})| \leq \mu$
2. Region within visible space but at distance greater than $\mu$: $\mu$
3. Non-visible points further than $\mu$: null

### Projective TSDF
- Easy to compute and trivially parallelizable
- Approximation but converges to SDF

Given raw depth map $R_k$ with known pose $\bm{T}_{g, k}$, for $\bm{p}_g$ in global frame, compute

$$
\Psi(\lambda^{-1} \|\bm{t}_{g, k} - \bm{p}_g\|_2 - R_k(\bm{x}))
$$

1. Project $\bm{p}_g$ into depth image coordinate to find closet pixel ($\lfloor . \rfloor$ denotes nearest neighbour lookup)
$$
\bm{x} = \lfloor \pi(\bm{K} \bm{T}_{g, k}^{-1} \bm{p}_g) \rfloor
$$
2. Distance to camera center
$$
\lambda = \|\bm{K}^{-1} \dot{\bm{x}} \|_2
$$
3. Distance between $\bm{p}_{g, k}$ and camera position in global frame
$$
\|\bm{t}_{g, k} - \bm{p}_g\|_2
$$
4. Compute projective signed distance
$$
\lambda^{-1} \|\bm{t}_{g, k} - \bm{p}_g\|_2 - R_k(\bm{x})
$$
5. Truncate signed distance as describe in [[TSDF representation|paper-read.kinect-fusion#tsdf-representation]]. Note its is scale so that non-trucated values before/after zero crossing are stored in discrete volume
$$
\Psi(\eta) =
\begin{cases}
\min(1, \frac{\eta}{\mu}) sgn(\eta) &\text{iff}\ \eta \geq -\mu \\
\text{null} &\text{otherwise}
\end{cases}
$$
6. $W_k(\bm{p}_g)$ is proportional to $\cos\theta / R_k(\bm{x})$, where $\theta$ is angle between ray and surface normal

### Global fusion

Apply [[Incremental calculation (combine depth image one by one)|epipolar-geometry.tsdf#incremental-calculation-combine-depth-image-one-by-one]] pointwise for $\{\bm{p}|F_k(\bm{p}) \neq \text{null}\}$ (unmeasurable region is not updated)
- Unit weight is good enough
- Truncate updated weight allow moving average reconstruction
- Fuse raw depth map as smoothed one removes the high frequency structure

## Surface Prediction from Ray Casting the TSDF
- Apply per pixel raycast. For ray $\bm{T}_{g, k} \bm{K}^{-1} \dot{\bm{u}}$, start at minimum depth and stops at zero crossing (front / back) or exit volume
- Surface normal is estimated with the numerical [[gradient of iso-surface|geometry.signed-distance-function#gradient-of-iso-surface]]
- March faster with trucated value as non-trucated value must exist before zero crossing, as it is guaranteed by trucate function scaling
- If $F^{+}_t$ and $F^{+}_{t + \Delta t}$ are at either side of zero crossing, approximate interpolation point with linear interpolation
$$
t^{*} = t - \frac{\Delta t F_t^{+}}{F^{+}_{t + \Delta t} - F_t^{+}}
$$ 

## TODO
- [ ] Is $\| \bm{K}^{-1} \dot{\bm{u}} \|_2 R_k(\bm{u})$ equivalent to $\|\bm{K}^{-1}[u R_k(\bm{u}), vR_k(\bm{u}), R_k(\bm{u})]^T\|_2$?
- [ ] Surface normal in fusion
- [ ] Min/max block acceleration
- [ ] Ray/trilinear cell intersection