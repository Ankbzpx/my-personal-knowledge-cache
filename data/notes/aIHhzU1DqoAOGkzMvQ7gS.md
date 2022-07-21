> Reference: https://stackoverflow.com/questions/849211/shortest-distance-between-a-point-and-a-line-segment

Compute distance from point p to line segment defined by w, w.
```
// compute projected distance by dot product
var t = p.sub(v).dot(w.sub(v).normalize());
// the crucial step, as we are dealing with segment here
t = Math.min(Math.max(t, 0), 1.0));
// compute the projected point
const proj = v.add(w.sub(v).normalize().mul(t);
// compute distance to projected point
const d = p.sub(proj).norm();
```