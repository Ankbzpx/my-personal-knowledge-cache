---
id: NaNIIvXTNnClxjHuidPPS
title: Parse JSON
desc: ''
updated: 1643437209903
created: 1643189432118
---

```
import json
json_data = json.load(open("json_file.json"))

# processing as dictionary
...

# save as a file
json.dump(json_data, open("json_file_edit.json", mode='w'))

# encode as string
json.dumps(json_data)
```
