---
id: HSxWXFhPQxcylUjCKd4la
title: Container
desc: ''
updated: 1647839673552
created: 1641262973688
---

## Array

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

## Vector

Continuous memory locations, number of elements can change
```
vector<int> iVec10;
vector<int> empty;
vector<int> iVec5(1, 2, 3, 4, 5);
int iArr3[] = {1, 2, 3};
vector<int> iVec3(iArr3, iArr3+3)

int average(vector<int> iVec){ // pass by copy, need local copy (Do not use in Google c++ style)
  ...
}

// do not change argument, pass by reference
int average(const vector<int> &iVec){
  ...
}

// intend to change argument, pass by pointer
int average(vector<int> *iVec){
  iVec->size();
  (*iVec)[0] = 1;
}

// pass temporary variable (rvalue reference)
int average(vector<int> &&iVec){
 ...
}
```

### Vector comparison
> Reference: https://en.cppreference.com/w/cpp/container/vector/operator_cmp

Similar to `std::lexicographical_compare. compare [lexicographically](https://en.wikipedia.org/wiki/Lexicographic_order) (generalized alphabetical order )
### Unroll nested vector
>Reference: https://stackoverflow.com/questions/20994321/clean-ways-to-write-multiple-for-loops

```
grid_dim = 10;
std::vector<int> grid_3d;
grid_3d.resize(grid_dim * grid_dim * grid_dim);

auto grid_query = [&](int, i, int, j, int, k){
  return grid_3d[(i * grid_dim + j) * grid_dim + k];
};
```