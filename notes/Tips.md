---
tags: [Others]
title: Tips
created: '2021-06-20T05:17:27.729Z'
modified: '2021-06-20T09:37:07.002Z'
---

# Tips

## Rotate landmarks in normalized coordinate
```
Image coordinate                  Normalized coordinate
0------------+------ img_w        0------------------+-----------1
|            |           |        |                  |           |
|            |           |        |                  |           |
|            |           |        |                  |           |
|            |           |        |                  |           |
|           y|           |        |               y_n|           |
|       +-------------+  |        |         +-----------------+  |
|       |     ^       |  | ======>|         |        ^        |  |
|   x   |    h|       |  |        |   x_n   |     h_n|        |  |
+------>|     v       |  |        +------>  |        v        |  |
|       +-------------+  |        |         +-----------------+  |
|       <------------->  |        |         <----------------->  |
|            w           |        |                 w_n          |
img_w -------------------+        1------------------------------+
```

- Move coordinate from top left to center
- Scale coordinate such that x, y axis units are equally scaled `x = x / img_w * img_h`
- Apply 2d rotation matrix $\begin{bmatrix}\cos(\theta) & \sin(\theta)\\-\sin(\theta) & \cos(\theta)\end{bmatrix}$
- Scale coordinate back `x = x / img_h * img_w`
- Move coordinate back from center to top left

## Adaptive threshold
- Apply [AlphaFiltering](https://en.wikipedia.org/wiki/Alpha_beta_filter) to raw data
- Apply [Welford's online algorithm](https://en.wikipedia.org/wiki/Algorithms_for_calculating_variance) to compute online mean and variance from filtered data (ignore first few observations until statistics stabilized)
- Detect using `mean +/- 3 * std` as threshold
Similar idea: https://stackoverflow.com/questions/22583391/peak-signal-detection-in-realtime-timeseries-data

## Tweak blink detection
```
  +--------(eye open)--------+
  |                          |
  v                          |
OPEN ---(eye close)---> CLOSE_WAIT
  ^                          |
  |                 (eye still close)
(eye still open)             |
  |                          v
OPEN_WAIT <--(eye open)-- CLOSE
  |                          ^
  |                          |
  +-------(eye close)--------+
```
- Apply adaptive threshold
- Track velocity to filter out untrustworthy data
- Add `WAIT` states for FSM
- Sync left/right eye by close/open both eye when all/one of them are in CLOSE_WAIT/OPEN_WAIT state (depends if ignore single blink)

## Classification override
Allow infrequent to override frequent class for more diverse results

```
// Row: query_key
// Cols: current_key
// visemeOverride[query_key][current_key] is query_key allowed to override current key
const visemeOverride: Boolean[][] = [[true, true, true, true, false], [false, true, true, true, false], [false, false, true, true, false], [false, false, false, true, false], [true, true, true, true, true]];

const visemeDuration: Viseme = { A: 4, E: 3, I: 2, O: 1, U: 5 }; // number of ticks each key is expected to last
```

