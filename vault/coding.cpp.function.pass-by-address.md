---
id: lqvhn0xsdQ7ev7cflCHUr
title: Pass by Address
desc: ''
updated: 1640016580949
created: 1640016573064
---

Pass address of argument, affect its value
```
void add(int *x1, int *x2){
  *x1 += *x2; // dereferenced to change value being pointed to
  *x1 = 0; // dereferenced to change value being pointed to
}
int a = 1;
int b = 2;
add(&a, &b); // a is 3, b is 0
```
