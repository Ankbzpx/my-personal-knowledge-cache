> Reference: https://rpg.ifi.uzh.ch/docs/teaching/2021/09_multiple_view_geometry_3.pdf

Random Sample Consensus (RANSAC) is used for robust estimation (with large potion of outliers)

> RANSAC is **non-deterministic**

## Steps
1. Sample minimum data samples required to fit the model and fit it
2. Compute cost/loss for each data point, mark the one supporting current model as inliers
3. Repeat $k$ times
4. Select the set with maximum number of inliers, use that model / fit new model using all inliers

## $k$ Selection

$$
1-p = (1-w^S)^K \\
k = \frac{\log(1-p)}{\log(1-w^S)}
$$

- $N$ total number of data points
- $w$ fraction of lnliers in dataset, the probability of selecting an inlier in dataset
- $S$ minimum points to fit the model
    - $w^S$ probability of all selected points are inliers
    - $1 - w^S$ probability of not all inliers
- $k$ RANSAC iteration executed so far
    - $(1-w^S)^K$ probability of not all inlier for all iteration executed
- $p$ probability of success