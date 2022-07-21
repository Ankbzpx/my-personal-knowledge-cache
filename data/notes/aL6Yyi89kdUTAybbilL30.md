
## Pillow
> need array conversion

```
from PIL import Image
import numpy as np

image = Image.open("image.png")
array_from_image = np.array(image)
image_from_array = Image.fromarray(array_from_image)
image_from_array.save("new_image.png")
```

## OpenCV
> can be directly indexed

```
import cv2
import numpy as np

image_bgr = cv2.imread("image.png", cv2.IMREAD_COLOR)
image_grayscale = cv2.imread("image.png", cv2.IMREAD_GRAYSCALE)
image_bgra = cv2.imread("image.png", cv2.IMREAD_UNCHANGED)

...

array = np.zeros((10, 10, 4), dtype=np.uint8)
cv2.imwrite("new_image.png", array)
```