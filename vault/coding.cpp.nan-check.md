---
id: 20d1qop5xyl6wpqnuwv85va
title: NaN Check
desc: ''
updated: 1649126538421
created: 1649126538421
---

## Use comparison
> Reference: https://stackoverflow.com/questions/570669/checking-if-a-double-or-float-is-nan-in-c

Comparisons involving `NaN` values are always false

`v != v`

> Not hold with `-ffast-math` flag

## C++ 11
Use `std::isnan()`