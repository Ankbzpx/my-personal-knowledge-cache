---
tags: [Coding]
title: Javascript/Typescript notes
created: '2021-06-20T03:54:22.179Z'
modified: '2021-06-20T09:37:50.400Z'
---

# Javascript/Typescript notes

## async/await

```
function f(): Promise<T>{
    return new Promise<T>(resolve -> {resolve(readfile...)}).then(()=>{return bar})
}
async function af(): Promise<T>{
    let bar = await f();
    ...
}
```

## Function binding
Use `bind` to avoid lose this when pass object method as callback

```
let object = {
  func(){}
}

object.func(); // func undefined
let bindfunc = object.func.bind(object);
object.func(); // successfully called
```
