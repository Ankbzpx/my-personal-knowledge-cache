---
id: 3nZ5hVIFAR7mFuktUTV0U
title: Commandline Arguments
desc: ''
updated: 1647412863847
created: 1643437038329
---
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