---
id: Wl0lwAJnoYz9ndonkeuW0
title: Pass Address by Reference
desc: ''
updated: 1640017159773
created: 1640017150177
---
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
