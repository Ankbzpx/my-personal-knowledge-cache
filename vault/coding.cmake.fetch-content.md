---
id: 2IowUFHgbNrRp6MQpgB1o
title: Fetch Content
desc: ''
updated: 1645514813909
created: 1645514705563
---

> cmake/gflags.cmake

```
if(TARGET gflags::gflags)
    return()
endif()

include(FetchContent)
FetchContent_Declare(
    gflags
    GIT_REPOSITORY https://github.com/gflags/gflags.git
    GIT_TAG v2.2.2
)
FetchContent_MakeAvailable(gflags)
```

> CMakeLists.txt

```
list(PREPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
include(gflags)
target_link_libraries(${PROJECT_NAME} PUBLIC gflags::gflags)
```

