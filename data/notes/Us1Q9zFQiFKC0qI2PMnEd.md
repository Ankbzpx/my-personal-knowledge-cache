
> MyClass.swift

```
let callbackTarget = UnsafeMutableRawPointer(Unmanaged.passUnretained(self).toOpaque())
oc_wrapper.myFunc(callback: {
    let coordinator = Unmanaged<MyClass>.fromOpaque(observer!).takeUnretainedValue()
    ...
}, callbackTarget)
```

> oc_wrapper.h

```
- (void)myFunc:(void(*)(int, void*))callback callbackTarget:(void*)callbackTarget;
```

> oc_wrapper.mm

```
- (void)myFunc:(void(*)(int, void*))callback callbackTarget:(void*)callbackTarget {
    _cpp->myFunc(callback, callbackTarget);
}
```

> CppClass.h

```
void myFunc(void(*callback)(int param, void*), void* callbackTarget);
```

> CppClass.cpp

```
void CppClass::myFunc(void(*callback)(int param, void*), void* callbackTarget) {
    callback(static_cast<int>(param), callbackTarget);
}
```
