---
id: RBq68Td2dQ3MmYPFJsYiF
title: Variable Declaration
desc: ''
updated: 1641265993724
created: 1641265853387
---

## Pointer
**pointer** is a variable that holds a memory address as its value.
```
int *iPtr; // a pointer to an integer value, int* iPtr; and int * iPtr
           //valid but not favorable
double *dPtr; // a pointer to a double value
int *iPtr1, *iPtr2; // declare two pointers to integer values
```

## Reference
**reference** is a variable acts as an alias to another variable
```
int v = 5; // normal integer
int &ref = v; // reference to variable v
v = 6; // v is now 6
ref = 7; // v is now 7
++ref; // v is 8 before processing current statement
ref++; // v is 8 after processing current statement
```

## Address-of Operator (&)
`&`: get memory address assigned to the variable
```
int x = 5;
std::cout << x << '\n'; // print value of x
std::cout << &x << '\n'; // print the memory address of x
```

## Dereference Operator (*)
`*`: get the value of a particular address
```
int x = 5;
std::cout << x << '\n'; // print value of x
std::cout << &x << '\n'; // print the memory address of x
std::cout << *&x << '\n'; // print value at the memory address of x
```

## Rvalue Reference
Temporary variables that doesn't have memory address
```
int a; //lvalue
int&& b = 5; //rvalue

a = b; // ok
//b = a; error
```