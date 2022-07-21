
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
