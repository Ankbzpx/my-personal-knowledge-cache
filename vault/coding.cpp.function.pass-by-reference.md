---
id: 2SeC43FbDbqvIE2ehdw7V
title: Pass by Reference
desc: ''
updated: 1640016546429
created: 1640016511280
---

Pass reference of variable, will affect the argument's value
```
void add(int &x1, int &x2){
  x1 += x2;
  x1 = 0;
}
int a = 1;
int b = 2;
add(a, b); // a is 3, b is 0
```
Google c++ style do **NOT** use pass by reference to modify arguments (use const reference)
```
void access(const int x){
  ...
}

int a = 1;
access(a); // a is 1
```