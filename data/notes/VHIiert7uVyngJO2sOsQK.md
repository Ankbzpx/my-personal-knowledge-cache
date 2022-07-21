
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