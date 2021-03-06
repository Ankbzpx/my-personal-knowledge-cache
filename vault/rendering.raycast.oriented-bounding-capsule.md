---
id: v7mUMqx1GGpznwe4HYSuk
title: Oriented Bounding Capsule (OBC)
desc: ''
updated: 1640019451719
created: 1640019431853
---

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
