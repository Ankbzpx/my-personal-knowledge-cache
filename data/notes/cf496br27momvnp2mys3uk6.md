
## Use pyclipper

```
from pyclipper import Orientation, scale_to_clipper

def is_CCW(polygon):
    return Orientation(scale_to_clipper(polygon))

```

## Numpy

> Reference: https://stackoverflow.com/questions/1165647/how-to-determine-if-a-list-of-polygon-points-are-in-clockwise-order

> FIXME: it keeps giving wrong results, figure out why?


```
import numpy as np

def is_CCW(coutour):
    start = coutour[1:]
    end = coutour[:-1]
    return np.sum((start[:, 0] - end[:, 0]) / (start[:, 1] + end[:, 1])) < 0
```