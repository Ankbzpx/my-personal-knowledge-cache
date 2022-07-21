
Use `bind` to avoid lose `this` when pass object method as callback
## Example 1
```
let object = {
  func(){}
}

object.func(); // func undefined
let bindfunc = object.func.bind(object);
object.func(); // successfully called
```

## Example 2
```
Class C {
  constructor() {
    this.message = "Class C message";
    // undefined
    this.addEventListener("mousedown", this.onMouseDown, false);
    // Class C message
    this.addEventListener("mousedown", this.onMouseDown.bind(this), false);
  }

  onMouseDown() {
    console.log(this.message);
  }
}
```