---
id: h37yDXiC9pZLK0yIlZAyc
title: Async Await
desc: ''
updated: 1640017633859
created: 1640017627132
---


```
function f(): Promise<T>{
    return new Promise<T>(resolve -> {resolve(readfile...)}).then(()=>{return bar})
}
async function af(): Promise<T>{
    let bar = await f();
    ...
}
```