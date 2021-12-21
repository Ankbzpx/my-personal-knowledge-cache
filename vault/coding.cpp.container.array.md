---
id: RQUDZDWu9Y4f2nWRwMmbg
title: Array
desc: ''
updated: 1640017209443
created: 1640017199803
---

Continuous memory locations, number of elements must be constant
```
double dArr[10];
int iArr[] = {1, 2, 3, 4, 5};
float fArr[3] = {1, 2} // omiteed element will be 0
dArr[0] = 5;
b = iArr[1] + 2;
fArr[2] = fArr[0] + fArr[1];
```
Array name stores the memory address of the first element (can be regarded as a pointer). Modify within the function will affects its values
```
int max(int[] a, int size){
  ...
}
int arr[] = {1, 2, 3};
int *aPtr;
aPtr = arr;
aPtr = &arr[0];
std::cout << max(arr, sizeof(arr)/sizeof(int));
```
