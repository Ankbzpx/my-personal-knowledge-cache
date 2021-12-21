---
id: Nz5pcos6le0MPPpivEW1r
title: Adaptive Threshold
desc: ''
updated: 1640018754741
created: 1640018747166
---

- Apply [AlphaFiltering](https://en.wikipedia.org/wiki/Alpha_beta_filter) to raw data
- Apply [Welford's online algorithm](https://en.wikipedia.org/wiki/Algorithms_for_calculating_variance) to compute online mean and variance from filtered data (ignore first few observations until statistics stabilized)
- Detect using `mean +/- 3 * std` as threshold
Similar idea: https://stackoverflow.com/questions/22583391/peak-signal-detection-in-realtime-timeseries-data