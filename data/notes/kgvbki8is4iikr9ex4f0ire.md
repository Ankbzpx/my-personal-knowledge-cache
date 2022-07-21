
> https://openaccess.thecvf.com/content/ICCV2021/papers/Zhang_Lightweight_Multi-Person_Total_Motion_Capture_Using_Sparse_Multi-View_Cameras_ICCV_2021_paper.pdf

## Single image / video

### Pose regression
> SMPL-X & Adam

- Direct regression from image to parameters
- Real time but not suitable for occlusion or multi-person
- No geometric correspondence with 2D joints

### Key-point detection
- Guarantee geometric alignment
- Need heavy post-processing
- Susceptible to self-occlusion

## Sparse Multi-view

### 4D Body Association

Sparse multi-view
- 2D keypoints -> triangulate 3D keypoints
- Tracked 3D point -> match 2D detection (Pnp?)

### Hand and Face Bootstrapping

- body skeleton -> 3D bounding sphere with constant radius (commensurate with physical size) -> project 2D bbox -> further refinement with hand detector -> refined 2D bbox
- For each refined 2D bbox (associated with body wrist), apply NMS using hand association score (crossmodality consistency score and cross-scale consistency score)
> **crossmodality consistency score** (pose-regression chirality ambiguity): 
> $$
avg(\max(0, 1 - \frac{\|\text{pose-regression position} - \text{keypoint-detection position}\|_2^2}{diag(RoI)}))$$
>
> **cross-scale consistency score** (wrist misalignment):
> $$
avg(\max(0,1 - \frac{\|\text{body skeleton wrist position} - \text{local wrist position}\|_2^2}{diag(RoI)}))$$

### Two-stage Parametric Fitting
- local intialization: 
    - hand - hand association score
    - body/head - SMPLify-X
- total optimization: l2 body 3D position distance + 2d projected keypoints distance + gesture range regularization

### Feedback
- limb detector
- re-render human model for visibility -> binary occupancy mask -> smoothed continuous occupancy mask -> occupancy probability of the person

## Questions
- [ ] 3D -> 2D projection (projection matrix with hnormalize? gradient backprop?)
- [ ] Body ground truth 3D?
- [ ] Hand association score for different views balancing (the parameter to refine?)
- [ ] Distance transformations for smoothing
- [ ] Human body parameterization

## TODO
- [ ] *SMPLify-X (regularization term)
- [ ] Adam
- [ ] 4D body association
- [ ] Mean Per Joint Position Error
- [ ] Heatmap-based detector
