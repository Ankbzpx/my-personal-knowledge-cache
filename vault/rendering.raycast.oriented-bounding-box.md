---
id: GgcYdhI0zk5zF514T9akT
title: Oriented Bounding Box (OBB)
desc: ''
updated: 1640019457787
created: 1640019411585
---

> Reference: http://www.opengl-tutorial.org/miscellaneous/clicking-on-objects/picking-with-custom-ray-obb-function/

```
fun intersectOBBox(scaledHalfExtend: Float3, modelMatrix: Mat4): Float? {
        // farthest near intersection distance
        var dMin = 0.0f
        // nearest far intersection distance
        var dMax = 100000.0f

        // vector from origin To OBB Center
        val delta = modelMatrix.position - origin

        // intersection check for two planes perpendicular to x/y/z axis separately
        listOf(modelMatrix.x.xyz, modelMatrix.y.xyz, modelMatrix.z.xyz).map { normalize(it) }.zip(listOf(scaledHalfExtend.x, scaledHalfExtend.y, scaledHalfExtend.z)).forEach { (axis, extend) ->
            val deltaProj = dot(axis, delta)
            val cosTheta = dot(direction, axis)

            if (abs(cosTheta) > 0.001f) {
                // distance to near intersection
                var dNear = (deltaProj - extend) / cosTheta
                // distance to far intersection
                var dFar = (deltaProj + extend) / cosTheta

                // swap if needed
                if (dNear > dFar) {
                    dNear = dFar.also { dFar = dNear }
                }

                dMin = dMin.coerceAtLeast(dNear)
                dMax = dMax.coerceAtMost(dFar)

                if (dMin > dMax) {
                    return null
                }
            } else {
                // ray perpendicular to plane normal a.k.a. parallel to plane
                // (ruichen): I don't understand logic here
                if (-deltaProj - extend > 0.0f || -deltaProj + extend < 0.0f) {
                    return null
                }
            }
        }
        return dMin
    }

```
