
## FIRST_FRAME
1. Consider the first frame as the first keyframe.

2. For the following frame, match 2D landmarks and compute average pixel distance w.r.t. first keyframe. If it is larger than a threshold, consider it as the second keyframe and start INITIALIZATION, otherwise go back to 2.

## INITIALIZATION
1. Given 2D to 2D landmarks correspondence, fit and decompose essential matrix. Replace $T_{rc}$ with IMU rotation. (Assuming the first keyframe pose as the world origin, cv fit/decompose essential matrix computes camera matrix $T_{cr}$, while we would like $T_{rc}$ for pose representation)

2. Initialize local map by triangulating 3D landmarks with poses and 2D to 2D landmarks correspondence. If valid 3D landmarks count is low, go back to FIRST_FRAME-2. (To triangulate, we need camera matrix $T_{1w}$ and $T_{2w}$, so the triangulated 3D landmarks are in world coordinate. We consider a 3D landmark validly triangulated only if it has low symmetric reprojection error and sufficient cosparallax).

## TRACKING
1. For the following frame, we match the 2D landmarks with 3D landmarks in local map, as well as a match ratio, indicating how well the tracking is. If match ratio is 0, tracking is lost, go back to FIRST_FRAME-1 (Caching current pose for initialization has not been implemented yet).

2. Compute $t_{wc}$ using IMU $R_{wc}$, obtain $T_{wc}$ . Verify if the translation is valid. If it's invalid, use last frame pose. If the translation is valid and match ratio is smaller than a threshold, we consider it a keyframe and redo INITIALIZATION-2. However, we scale the translation, and reject if valid 3D landmarks count is low and continue TRACKING-1.