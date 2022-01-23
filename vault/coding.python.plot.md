---
id: Uv44sZa8u8bSLxHVy0FYo
title: Plot
desc: ''
updated: 1642932150987
created: 1642931993783
---

## Equal axis

```
import numpy as np
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 10))
plt.axis('equal')
plt.plot(np.arange(20), np.arange(10, step=0.5))
```