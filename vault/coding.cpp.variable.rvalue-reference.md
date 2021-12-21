---
id: 18Ixdalqdfjkyzbvq1ivA
title: Rvalue Reference
desc: ''
updated: 1640016428481
created: 1640016417672
---

Temporary variables that doesn't have memory address
```
int a; //lvalue
int&& b = 5; //rvalue

a = b; // ok
//b = a; error
```
