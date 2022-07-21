## Capture

### No capture
```
auto functor = [](int i){
    return i * i;
}
```

### Read only
```

std::vector<int> vec(1);
auto functor = [vec](int i){
    return i * vec[0];
}
```

### Mutable
```
std::vector<int> vec(1);
auto functor = [&vec](int i){
    vec[0] = i;
}
```

### All capture
```
std::vector<int> vec(1);
auto functor = [&](int i){
    vec[0] = i;
    return i * vec[0];
}
```

## Pass Lambda as function argument

>**IMPORTANT** Function pointer `void(*func)()` doesn't capture states, use `std::function<void()> func` instead 
>
> Reference: https://stackoverflow.com/questions/28573986/how-to-pass-a-lambda-in-a-function-with-a-capture

```
auto functor = [](int i){
    return i * i;
}
```

```
void func (std::function<int(int)> callback) {
    callback(1);
}
```

```
func(functor);
```
