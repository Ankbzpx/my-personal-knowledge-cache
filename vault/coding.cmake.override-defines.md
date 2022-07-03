---
id: aony7mb0rwpm9pceph32bpl
title: Override Defines
desc: ''
updated: 1656827492476
created: 1656826999877
---
> Reference: https://stackoverflow.com/questions/32947974/file-path-with-cmake-add-definitions


> CMakeList.txt

```
target_compile_definitions(target PRIVATE DATA_DIR="${DATA_DIR}/${DATA_DIR}")
```

> main.cpp

```
// Use Macro as string literals (https://stackoverflow.com/questions/798221/c-macros-to-create-strings)
std::string path = DATA_DIR "/data.txt"
```