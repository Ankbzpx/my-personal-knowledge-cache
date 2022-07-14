---
id: e9ow9c6apojiqfio5e5ceht
title: Project Starline A high-fidelity telepresence system
desc: ''
updated: 1657801348491
created: 1657801332149
---

## HCI
VR, AR: hard to capture under headset, insufficient pixel density and viewport

autostereoscopic display (65-inch 8K 33.1M pixel 60Hz, 1.25m eye-to-eye distance, 45 pixel per degree)

- Face near display panel (reduce disparity)
- Place middle wall to block bottom of display (reduce depth conflicts)
- Matching remote to local transform for both sides (ensure mutual eye contact)

### TODO
- [ ] crosstalk
- [ ] vergence-accommodation conflict
- [ ] disparity
- [ ] depth conflicts
- [ ] mutual eye contact

## Setup
- 4 tracking cameras (2 above, 2 side): localized eyes, ears, mouth, 120Hz
- 3 depth (stereo) camera: depth stream, 180Hz -> image-based depth fusion
- 4 color camera: color stream, 60Hz -> Normal based texture blending

### TODO
- [ ] HRTF (head-related transfer function)
- [ ] Normal based texture blending

## Implementation

### Lighting
- IBL (Image-based rendering), no illumination or reflectance
- Interpolates textures from 4 color cameras
- Non-Lambertian reflectance rander incorrectly under non-diffuse lighting (mitigated using soft lighting)
- illumination nonuniformity (stronger intensities for display unit near wall)

### Calibration
- Minimizing reprojection error over planar targets
- Color calibration to Illuminant D65 (gain, color correction matrix, gamma)

### Capture
- Capture sensor placed at periphery of the display, large parallax
- Near-infrared (NIR) global shutter image sensors
- Customized depth sensor with near depth continuities
- 3 types of NIR illumination (NIR bounce light, NIR spot light, illuminate back wall)

### 3D Face Tracking
- Eye location -> stereo rendering
- Mouth & ear location -> spatial audio rendering + crosstalk cancellation
- 34 facial landmarks, determine 5 2D features (eyes, mouth, ears) and triangulated into 3D
- Latency causes crosstalk, mitigated by extrapolation + double exponential smoothing + hysteresis filter

### Compression and transmission
- Transmit color (8 bit) and depth (10 bit) images via video compression (NVENC/NVDEC unit, H.256 codec, YUV420 chroma subsampling)
- Clamp depth to reduce quantization artifacts
- Transmit via WebRTC

### Rendering
- Compute shadow map from color cameras by raycasting on input depth map
- Compute user view output depth maps by raycasting on input depth map
- Use shadow map to weighted color blend output depth map

#### Traditional Surface fusion
- Fuse depth view into TSDF as volumetric grid, weighting depth pixel based on depth gradient
- March along rays into precomputed voxel grid, find root (iso surface)

#### Novel raycast approach
- Rasterizing input depth view to low-res output view, find lower/upper bound of distance along the ray using 2D min/max filter, dilate it with small margin
- For any point on user view ray, for each depth images, transform point to depth camera view coordinate, sample depth and fusion weight
- Fusion weight is inverse proportional to depth standard deviation over 7x7 pixel neighbourhood (0 if point is outside viewport)

#### Weighted color blending
- Compute partial visibility using percentage close filter on shadow map
- Modulate blend weight by squared cosine of angle between surface normal and camera vector (?)
- Adaptively blur composite image along depth discontinuities (Edge blending)

### Audio
[skip for now]

### TODO
- [ ] Non-Lambertian reflectance
- [ ] Non-diffuse lighting
- [ ] Lighting ratio
- [ ] Non-planar warp
- [ ] ESPReSSo
- [ ] NIR illumination
- [ ] Hysteresis filter
- [ ] NVENC & NVDEC
- [ ] TSDF (depth gradient weighting)
- [ ] [2D min/max filter](http://www.code-spot.co.za/2011/01/24/2d-minimum-and-maximum-filters-algorithms-and-implementation-issues/)
- [ ] Bisection search
- [ ] Relief texture mapping
- [ ] [Percentage closer filter](https://developer.nvidia.com/gpugems/gpugems/part-ii-lighting-and-shadows/chapter-11-shadow-map-antialiasing)