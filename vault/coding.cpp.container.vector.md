---
id: bCGz2Hik1tjjS5qZpGOC1
title: Vector
desc: ''
updated: 1640017226281
created: 1640017215199
---

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