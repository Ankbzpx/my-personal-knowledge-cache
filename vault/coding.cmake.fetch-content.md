---
id: 2IowUFHgbNrRp6MQpgB1o
title: Fetch Content
desc: ''
updated: 1653962691964
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
> Reference: https://cmake.org/cmake/help/latest/module/ExternalProject.html#command:externalproject_add

### Pass CMake flag
```
FetchContent_Declare(
    depname
    ...
)
FetchContent_GetProperties(depname)
if (NOT depname_POPULATED)
    FetchContent_Populate(depname)
    set(CMAKE_BUILD_TYPE Release)
    add_subdirectory(${depname_SOURCE_DIR} ${depname_BINARY_DIR})
endif()

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