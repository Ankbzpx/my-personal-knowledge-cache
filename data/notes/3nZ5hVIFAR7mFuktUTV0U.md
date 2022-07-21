## Pass JSON string
Add single quotation mark `'` `'` so string won't be splitted.

## Blender

`blender -b -P script.py -- arg0 arg1`

> script.py

```
argv = sys.argv
argv = argv[argv.index("--") + 1:]
arg0 = argv[0]
arg1 = argv[1]
```