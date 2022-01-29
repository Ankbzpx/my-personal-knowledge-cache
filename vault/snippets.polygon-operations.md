---
id: DXryQkH15go9fC8P3uCYQ
title: Polygon Operations
desc: ''
updated: 1643287582879
created: 1642932269870
---
> Workflow: Simplify -> Smooth + resample -> Simplify -> Offset

## Polygon simplify
[Ramer–Douglas–Peucker & Visvalingam-Whyatt](https://github.com/urschrei/simplification)

## Polygon offset
[Clipper library](http://www.angusj.com/delphi/clipper.php)
([python binding](https://github.com/fonttools/pyclipper))


## Polygon smooth
[Bezier Curves Interpolation](https://web.archive.org/web/20131027060328/http://www.antigrain.com/research/bezier_interpolation/index.html#PAGE_BEZIER_INTERPOLATION)

My numpy implementation:
```
def bezier_curve_interpolation(polygon, smooth_factor = 1.0, resample_count = 8):
    """
    Bezier_curve_interpolation.
    
    Parameters
    ----------
    polygon : n x 2 numpy array
    smooth_factor : the larger the value, more close the curve is to the edge
    resample_count : number of samples for each bezier segment
    """

    def bezier_curve(cpts, t):
        tile = lambda x: np.tile(np.expand_dims(x, -1), len(t))
        pt0 = tile(cpts[0])
        pt1 = tile(cpts[1])
        pt2 = tile(cpts[2])
        pt3 = tile(cpts[3])
        return np.power(1 - t, 3) * pt0 + 3 * np.power(1 - t, 2) * t * pt1 + 3 * (1 - t) * np.power(t, 2) * pt2 + np.power(t, 3) * pt3
    
    polygon_next = np.roll(polygon, -1, axis=0)
    polygon_last = np.roll(polygon, 1, axis=0)
    mid_point = (polygon + polygon_next) / 2

    dist_to_last = np.linalg.norm(polygon - polygon_last, axis=1)
    dist_to_next = np.linalg.norm(polygon - polygon_next, axis=1)
    dist_sum = smooth_factor * (dist_to_next + dist_to_last)
    dir = np.roll(mid_point, 1, axis=0) - mid_point

    cpt_left = polygon + dir * (dist_to_last / dist_sum).reshape(-1, 1)
    cpt_right = polygon - dir * (dist_to_next / dist_sum).reshape(-1, 1)

    control_points = np.array([polygon, cpt_right, np.roll(cpt_left, -1, axis=0), polygon_next])
    return bezier_curve(control_points, np.linspace(0, 1, resample_count + 1)[:-1]).transpose((0, 2, 1)).reshape(-1, 2)
```
