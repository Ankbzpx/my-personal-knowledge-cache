---
id: VHIiert7uVyngJO2sOsQK
title: Subdirectory
desc: ''
updated: 1645512184869
created: 1645512012378
---

folder
 |
 |---CMakeLists.txt
 |
 |---subfolder
        |
        |---CMakeLists.txt

Add
```
include_directories(subfolder)
add_subdirectory(subfolder)
```
to `folder/CMakeLists.txt`, don't forget to `target_link_libraries`