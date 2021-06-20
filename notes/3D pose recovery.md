---
tags: [Matrix Transformation]
title: 3D pose recovery
created: '2021-06-20T09:03:27.617Z'
modified: '2021-06-20T09:37:45.335Z'
---

# 3D pose recovery

## Pipline
### FIRST_FRAME
1. Consider the first frame as the first keyframe.

2. For the following frame, match 2D landmarks and compute average pixel distance w.r.t. first keyframe. If it is larger than a threshold, consider it as the second keyframe and start INITIALIZATION, otherwise go back to 2.

### INITIALIZATION
1. Given 2D to 2D landmarks correspondence, fit and decompose essential matrix. Replace $T_{rc}$ with IMU rotation. (Assuming the first keyframe pose as the world origin, cv fit/decompose essential matrix computes camera matrix $T_{cr}$, while we would like $T_{rc}$ for pose representation)

2. Initialize local map by triangulating 3D landmarks with poses and 2D to 2D landmarks correspondence. If valid 3D landmarks count is low, go back to FIRST_FRAME-2. (To triangulate, we need camera matrix $T_{1w}$ and $T_{2w}$, so the triangulated 3D landmarks are in world coordinate. We consider a 3D landmark validly triangulated only if it has low symmetric reprojection error and sufficient cosparallax).

### TRACKING
1. For the following frame, we match the 2D landmarks with 3D landmarks in local map, as well as a match ratio, indicating how well the tracking is. If match ratio is 0, tracking is lost, go back to FIRST_FRAME-1 (Caching current pose for initialization has not been implemented yet).

2. Compute $t_{wc}$ using IMU $R_{wc}$, obtain $T_{wc}$ . Verify if the translation is valid. If it's invalid, use last frame pose. If the translation is valid and match ratio is smaller than a threshold, we consider it a keyframe and redo INITIALIZATION-2. However, we scale the translation, and reject if valid 3D landmarks count is low and continue TRACKING-1.

## Recovery translation given IMU rotation and 3D to 2D correspondence
### Algorithm
Given 3D landmark $\bm{X} = (x, y, z)$ corresponds to 2D landmark $\bm{x} = (x, y)$, camera intrinsic matrix $\bm{K} = \begin{bmatrix} f_x & 0 & c_x \\ 0 & f_y & c_y \\ 0 & 0 & 1 \end{bmatrix}$, it satisfies:
$$\bm{K}[\bm{R}|\bm{t}]\bm{X_h} = \bm{K}(\bm{R} \bm{X} + \bm{t})=\bm{x_h}$$
where $[\bm{R}|\bm{t}]$ is camera intrinsic matrix, $\bm{x_h}$ is homogeneous coordinate of $\bm{x}$, $\bm{X_h}$ is homogeneous coordinate of $\bm{X}$

Assume $\bm{R}$ is known from IMU, let
$$\bm{R} \bm{X} + \bm{t} = \begin{bmatrix} a_1 \\ a_2 \\  a_3 \end{bmatrix} + \begin{bmatrix} t_1 \\ t_2 \\  t_3 \end{bmatrix}, {\bm{K}}^{-1}\bm{x_h} = \begin{bmatrix} b_1 \\ b_2 \\ 1 \end{bmatrix}$$

We have
$$\cfrac{t_1 + a_1}{t_3 + a_3}=b_1, \cfrac{t_2 + a_2}{t_3 + a_3}=b_2$$

With matrix form
$$\begin{bmatrix} 1 & 0 & -b_1 \\ 0 & 1 & -b_2 \end{bmatrix} \begin{bmatrix} t_1 \\ t_2 \\ t_3  \end{bmatrix} = \begin{bmatrix} -a_1 + b_1a_3 \\ -a_2 + b_2 a_3 \end{bmatrix}$$

It has the form $\bm{A} \bm{t} = \bm{y}$, and can be solved via DLS $\bm{t} = {({\bm{A}}^{T} \bm{A})}^{-1} {\bm{A}}^{T} \bm{y}$ given sufficient points (2 equation for 1 point, needs at least 2 points for 3 unknown)

### DLS implementation
```
void SolveTranslationDLS(const std::vector<cv::Point3f>& v_p3_matched,
                         const std::vector<cv::Point2f>& v_p2_matched,
                         const Eigen::Matrix3d& K_inv,
                         const Sophus::Matrix3d& gt_R,
                         Sophus::Vector3d* est_t) {
  assert(v_p3_matched.size() == v_p2_matched.size() &&
         v_p3_matched.size() >= 0);

  Eigen::MatrixXd A(2 * v_p3_matched.size(), 3);
  Eigen::MatrixXd y(2 * v_p2_matched.size(), 1);
  for (size_t i = 0; i < v_p3_matched.size(); i++) {
    const Sophus::Vector3d X(v_p3_matched[i].x, v_p3_matched[i].y,
                             v_p3_matched[i].z);
    const Sophus::Vector2d x(v_p2_matched[i].x, v_p2_matched[i].y);

    const Sophus::Vector3d a = gt_R * X;
    const Sophus::Vector2d b = (K_inv * x.homogeneous()).hnormalized();

    A.row(2 * i) << 1, 0, -b.x();
    A.row(2 * i + 1) << 0, 1, -b.y();

    y.row(2 * i) << -a.x() + b.x() * a.z();
    y.row(2 * i + 1) << -a.y() + b.y() * a.z();
  }
  *est_t = (A.transpose() * A).inverse() * A.transpose() * y;
}
```
### RANSAC implementation
```
bool SolveTranslationRANSAC(const std::vector<cv::Point3f>& v_p3_matched,
                            const std::vector<cv::Point2f>& v_p2_matched,
                            const cv::Mat& K_mat, const Sophus::Matrix3d& gt_R,
                            Sophus::Vector3d* t, double threshold) {
  Eigen::Matrix3d K;
  cv::cv2eigen(K_mat, K);
  auto K_inv = K.inverse();

  assert(v_p3_matched.size() == v_p2_matched.size());

  std::vector<int> v_idx;
  v_idx.reserve(v_p3_matched.size());
  for (int i = 0; i < v_p3_matched.size(); i++) {
    v_idx.push_back(i);
  }

  Sophus::Vector3d est_t;

  if (v_p3_matched.size() < 2) {
    LOG(ERROR) << "Insufficient 3d-2d matches";
    return false;
  } else if (v_p3_matched.size() == 2) {
    LOG(INFO) << "Only 2 matches available, may produce wrong results";
    SolveTranslationDLS(v_p3_matched, v_p2_matched, K_inv, gt_R, &est_t);
  } else {
    std::vector<int> best_inliers_indices;
    uint iters = ComputeRansacIteration(0, v_p3_matched.size(), 2);

    uint i;
    for (i = 0; i < iters; i++) {
      // random sample 4 points to fit a model
      // Fixed random seed for reproducibility
      std::shuffle(std::begin(v_idx), std::end(v_idx), std::mt19937(0));

      Sophus::Vector3d tmp_t;
      std::vector<cv::Point3f> v_p3_tmp = {v_p3_matched[v_idx[0]],
                                           v_p3_matched[v_idx[1]]};
      std::vector<cv::Point2f> v_p2_tmp = {v_p2_matched[v_idx[0]],
                                           v_p2_matched[v_idx[1]]};

      SolveTranslationDLS(v_p3_tmp, v_p2_tmp, K_inv, gt_R, &tmp_t);

      std::vector<int> inlier_indices;

      // Evaulate results over whole dataset
      for (int j = 0; j < v_p3_matched.size(); j++) {
        Sophus::Vector3d X(v_p3_matched[j].x, v_p3_matched[j].y,
                           v_p3_matched[j].z);
        Sophus::Vector2d x(v_p2_matched[j].x, v_p2_matched[j].y);

        Sophus::Vector2d x_proj = (K * (gt_R * X + tmp_t)).hnormalized();

        if ((x_proj - x).norm() < threshold) {
          inlier_indices.push_back(j);
        }
      }

      if (inlier_indices.size() > best_inliers_indices.size()) {
        best_inliers_indices = std::move(inlier_indices);
        est_t = tmp_t;
        iters = ComputeRansacIteration(best_inliers_indices.size(),
                                       v_p3_matched.size(), 2);
      }
    }

    LOG(INFO) << "DLT ransac inlier size: " << best_inliers_indices.size();

    if (best_inliers_indices.size() < 2) {
      LOG(INFO) << "Inlier after ransac less than 2";
      return false;
    } else if (best_inliers_indices.size() == 2) {
      LOG(INFO) << "Only 2 points available, may produce wrong results";
    } else {
      // compute least square solution for all inliers
      std::vector<cv::Point3f> v_p3_inliers;
      std::vector<cv::Point2f> v_p2_inliers;

      v_p3_inliers.reserve(best_inliers_indices.size());
      v_p2_inliers.reserve(best_inliers_indices.size());

      for (int idx : best_inliers_indices) {
        v_p3_inliers.push_back(v_p3_matched[idx]);
        v_p2_inliers.push_back(v_p2_matched[idx]);
      }

      SolveTranslationDLS(v_p3_inliers, v_p2_inliers, K_inv, gt_R, &est_t);
    }
  }

  *t = -gt_R.transpose() * est_t;

  return true;
}
```
