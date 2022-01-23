---
id: MnMuRCJJCq6WddUMDRTVf
title: Function Binding
desc: ''
updated: 1640017653605
created: 1640017647444
---

Use `bind` to avoid lose this when pass object method as callback

```
let object = {
  func(){}
}

object.func(); // func undefined
let bindfunc = object.func.bind(object);
object.func(); // successfully called
```
