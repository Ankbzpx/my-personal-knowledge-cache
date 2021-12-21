---
id: LgEFfW0Ac5Yv5HEYGKyqk
title: Pass by Value
desc: ''
updated: 1640018988654
created: 1640016469133
---

Values of arguments are firstly copied, with copied values passed to the function, which doesn't change arguments' values
```
void add(int x1, int x2){
  x1 += x2;
  x2 = 0;
}
int a = 1;
int b = 2;
add(a, b); // a is still 1, b is still 2
```
