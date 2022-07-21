
## Equal axis

```
import numpy as np
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 10))
plt.axis('equal')
plt.plot(np.arange(20), np.arange(10, step=0.5))
```