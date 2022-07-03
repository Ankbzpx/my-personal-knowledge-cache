---
id: 2IowUFHgbNrRp6MQpgB1o
title: Fetch Content
desc: ''
updated: 1654583384145
created: 1645514705563
---

> Reference: https://cmake.org/cmake/help/latest/module/FetchContent.html

## CMake project

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

> **IMPORTANT** The module name can be arbitrary but the library name in `target_link_libraries` depends on the namespace of the library

## Non-CMake project
> cmake/imgui.cmake

```
if(TARGET imgui::imgui)
    return()
endif()

include(FetchContent)
FetchContent_Declare(
    imgui
    GIT_REPOSITORY https://github.com/ocornut/imgui.git
    GIT_TAG v1.87
)
FetchContent_MakeAvailable(imgui)

set(IMGUI_HDRS
    ${imgui_SOURCE_DIR}/imconfig.h
    ${imgui_SOURCE_DIR}/imgui.h
)

set(IMGUI_SRCS
    ${imgui_SOURCE_DIR}/imgui.cpp
    ${imgui_SOURCE_DIR}/imgui_demo.cpp
    ${imgui_SOURCE_DIR}/imgui_draw.cpp
    ${imgui_SOURCE_DIR}/imgui_tables.cpp
    ${imgui_SOURCE_DIR}/imgui_widgets.cpp
)

add_library(imgui STATIC ${IMGUI_HDRS} ${IMGUI_SRCS})
target_include_directories(imgui PUBLIC ${imgui_SOURCE_DIR})
target_compile_definitions(imgui PUBLIC
    IMGUI_IMPL_OPENGL_LOADER_GLAD
)
```
> CMakeLists.txt

```
list(PREPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
include(imgui)
target_link_libraries(${PROJECT_NAME} PUBLIC imgui)
```

## Auxiliary
### Pass define
```
FetchContent_Declare(
    depname
    ...
)
FetchContent_MakeAvailable(depname)

target_compile_definitions(depname INTERFACE
    DEPNAME_DEFINE
    CMAKE_BUILD_TYPE=Release
)
```

### Ignore git submodule
```
FetchContent_Declare(
    ...
    GIT_SUBMODULES ""
)
```

### Apply patch
```
FetchContent_Declare(
    ...
    PATCH_COMMAND patch -p1 ${CMAKE_CURRENT_SOURCE_DIR}/fix.patch
)
```

## TODO
- [ ] Library namespace