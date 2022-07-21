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