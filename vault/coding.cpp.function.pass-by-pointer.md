---
id: 0h1Xi2W0ozrlZgi5zvhPC
title: Pass by Pointer
desc: ''
updated: 1640016499220
created: 1640016489115
---

Pass pointer holds memory address of variable, will affect argument's value
```
void add(int *x1, int *x2){
  *x1 += *x2; // dereferenced to change value being pointed to
  *x1 = 0; // dereferenced to change value being pointed to
}
int a = 1;
int b = 2;
int *aPtr, *bPtr;
aPtr = &a;
bPtr = &b;
add(aPtr, bPtr); // a is 3, b is 0
```