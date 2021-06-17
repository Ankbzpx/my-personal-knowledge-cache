---
tags: [Coding]
title: C++ notes
created: '2021-06-17T02:44:36.374Z'
modified: '2021-06-17T03:00:22.762Z'
---

# C++ notes

## Variable

### Address-of operator (&)
`&`: get memory address assigned to the variable
```
int x = 5;
std::cout << x << '\n'; // print value of x
std::cout << &x << '\n'; // print the memory address of x
```
### Dereference operator (*)
`*`: get the value of a particular address
```
int x = 5;
std::cout << x << '\n'; // print value of x
std::cout << &x << '\n'; // print the memory address of x
std::cout << *&x << '\n'; // print value at the memory address of x
```
### Pointer
**pointer** is a variable that holds a memory address as its value.
```
int *iPtr; // a pointer to an integer value, int* iPtr; and int * iPtr; are valid but not favorable
double *dPtr; // a pointer to a double value
int *iPtr1, *iPtr2; // declare two pointers to integer values
```
### Reference
**reference** is a variable acts as an alias to another variable
```
int v = 5; // normal integer
int &ref = v; // reference to variable v
v = 6; // v is now 6
ref = 7; // v is now 7
++ref; // v is 8 before processing current statement
ref++; // v is 8 after processing current statement
```
### Rvalue references
Temporary variables that doesn't have memory address
```
int a; //lvalue
int&& b = 5; //rvalue

a = b; // ok
//b = a; error
```

## Function
### Pass by value
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
### Pass by pointer
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
### Pass by reference
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
Google c++ style do not use pass by reference to modify arguments (use const reference)
```
void access(const int x){
  ...
}

int a = 1;
access(a); // a is 1
```
### Pass by address
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
### Pass address by reference
Change address of argument within the function
```
void SetToNull(int *&ptr){ // reference to pointer
  ptr = nullptr;
}
int five = 5;
int *ptr = &five; // pointer holds address of five
std::cout << *ptr; // dereference, print 5, the value at address of five
SetToNull(ptr); // pointer changed to null pointer
```
In Google c++ style, use ** to dereference twice

## Container
### Array
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
### Vector
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
