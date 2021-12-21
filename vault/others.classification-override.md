---
id: JFuX7ZsYR764TWYQ2elyO
title: Classification Override
desc: ''
updated: 1640018858290
created: 1640018851690
---

Allow infrequent to override frequent class for more diverse results

```
// Row: query_key
// Cols: current_key
// visemeOverride[query_key][current_key] is query_key allowed to override current key
const visemeOverride: Boolean[][] = [[true, true, true, true, false], [false, true, true, true, false], [false, false, true, true, false], [false, false, false, true, false], [true, true, true, true, true]];

const visemeDuration: Viseme = { A: 4, E: 3, I: 2, O: 1, U: 5 }; // number of ticks each key is expected to last