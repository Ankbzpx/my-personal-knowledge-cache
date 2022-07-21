## Pass by Value

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

## Pass by Pointer

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

## Pass by Reference

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

## Pass by Address

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

## Pass Address by Reference

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

