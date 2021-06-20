---
tags: [Rendering]
title: Raycast
created: '2021-06-20T09:24:25.334Z'
modified: '2021-06-20T09:37:12.718Z'
---

# Raycast

## Oriented Bounding Box (OBB)
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

## Oriented bounding capsule (OBC)
> Reference: https://iquilezles.org/www/articles/intersectors/intersectors.htm
```
fun hitTestOrientedBoundingCapsule(capsule: Capsule): Boolean {
        val ao = capsule.pa - origin
        val bo = capsule.pb - origin

        val a = dot(ao, direction)
        val b = dot(bo, direction)

        return sqrt(dot(ao, ao) - a * a) <= capsule.ra || sqrt(dot(bo, bo) - b * b) <= capsule.ra
    }

    fun intersectOrientedBoundingCapsule(capsule: Capsule): Float? {
        val pa = capsule.pa
        val pb = capsule.pb
        val ra = capsule.ra
        val ro = origin
        val rd = direction

        val ba = pb - pa
        val oa = ro - pa
        val baba = dot(ba, ba)
        val bard = dot(ba, rd)
        val baoa = dot(ba, oa)
        val rdoa = dot(rd, oa)
        val oaoa = dot(oa, oa)
        val a = baba - bard * bard
        var b = baba * rdoa - baoa * bard
        var c = baba * oaoa - baoa * baoa - ra * ra * baba
        var h = b * b - a * c
        if (h >= 0f) {
            val t = (-b - sqrt(h)) / a
            val y = baoa + t * bard
            // body
            if (y> 0f && y <baba) return t
            // caps
            val oc = if (y <= 0f) oa else ro - pb
            b = dot(rd, oc)
            c = dot(oc, oc) - ra * ra
            h = b * b - c
            if (h> 0f) {
                return -b - sqrt(h)
            }
        }
        return null
    }

```
