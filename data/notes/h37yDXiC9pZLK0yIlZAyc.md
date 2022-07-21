

```
function f(): Promise<T>{
    return new Promise<T>(resolve -> {resolve(readfile...)}).then(()=>{return bar})
}
async function af(): Promise<T>{
    let bar = await f();
    ...
}
```
