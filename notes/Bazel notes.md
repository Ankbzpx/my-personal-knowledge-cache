---
tags: [Coding]
title: Bazel notes
created: '2021-06-20T03:49:31.026Z'
modified: '2021-06-20T05:00:18.080Z'
---

# Bazel notes

## glog
- Logging: `GLOG_alsologtostderr=1` shows all `LOG(severity_level)` logs, where `severity_level = {INFO, WARNING, ERROR, FATAL}`. Note `FATAL` will terminate the program
- Verbose logging: `GLOG_v=level` shows all `VLOG(level)` logs less or equal to specified level  

## External package
- `http_archive`: `BUILD` or patch, `load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")`
- `git_repository`: commit, patch, `load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")`
- `new_git_repository`: commit, `BUILD` or patch,`load("@bazel_tools//tools/build_defs/repo:git.bzl", "new_git_repository")`

### Package with native bazel support
```
http_archive(
    name = "com_github_gflags_gflags",
    sha256 = "34af2f15cf7367513b352bdcd2493ab14ce43692d2dcd9dfc499492966c64dcf",
    strip_prefix = "gflags-2.2.2",
    urls = ["https://github.com/gflags/gflags/archive/v2.2.2.tar.gz"],
)
```
`strip_prefix` is the name of the root folder of the archive

### Package without native bazel support - using `build_file`
It is recommended if package doesn't require external dependencies or they can be added similarly in `WORKSPACE`
```
# Animation compression is a fundamental aspect of modern video game engines. 
# Not only is it important to keep the memory footprint down but it is also critical to keep the animation clip sampling performance fast.
http_archive(
    name = "com_github_nfrechette_acl",
    build_file = "@//third_party:acl.BUILD",
    strip_prefix = "acl-2.0.0",
    type = "zip",
    sha256 = "eae362d5a4c7d5bc6f82b5c41efc66d968066c5145c414b92d26f53ce689145f",
    urls = ["https://github.com/nfrechette/acl/archive/refs/tags/v2.0.0.zip"]
)
```
`Build ` example
```
# third_party/acl.BUILD
cc_library(
    name = "acl",
    hdrs = glob(["includes/**/*.h"]),
    srcs = glob(["includes/**/*.h"]),
    includes = ["includes"],
    visibility = ["//visibility:public"],
)
```
reference it by `"@com_github_nfrechette_acl//:acl"`, or combine it with its dependencies

```
# third_party/BUILD

cc_library(
    name = "acl",
    visibility = ["//visibility:public"],
    deps = [
        ":rtm",
        "@com_github_nfrechette_acl//:acl",
    ],
)

cc_library(
    name = "rtm",
    visibility = ["//visibility:public"],
    deps = [
        "@com_github_catchorg_Catch2//:catch2",
        "@com_github_google_benchmark//:benchmark",
        "@com_github_nfrechette_rtm//:rtm",
    ],
)
```
and reference by `"//third_party:acl"`

### Package without native bazel support - using `patches`

This is recommended if package has complicated/nested dependencies that cannot be easily extracted into `WORKSPACE`

```
# GTSAM - GTSAM is a library of C++ classes that implement smoothing and mapping (SAM) in robotics and vision, 
# using factor graphs and Bayes networks as the underlying computing paradigm rather than sparse matrices.
http_archive(
    name = "com_github_borglab_gtsam",
    patch_args = [
        "-p1",
    ],
    patches = [
        "@//third_party:com_github_borglab_gtsam_fixes.diff",
    ],
    sha256 = "eaa561749edf7a2d402981828253e28aed6c717dae35738301c5ab23e2595f25",
    strip_prefix = "gtsam-4.0.3",
    urls = [
        "https://github.com/borglab/gtsam/archive/4.0.3.tar.gz",
    ],
)
```
Patch example (generated using git diff)
```
# third_party/com_github_borglab_gtsam_fixes.diff

diff --git a/BUILD b/BUILD
new file mode 100644
index 000000000..c29bc0337
--- /dev/null
+++ b/BUILD
@@ -0,0 +1,52 @@
+load("//:gtsam.bzl", "gtsam_library")
+
+cc_library(
+    name = "GKlib",
+    srcs = glob(["gtsam/3rdparty/metis/GKlib/*.c"]),
+    hdrs = glob(["gtsam/3rdparty/metis/GKlib/*.h"]),
+    includes = ["gtsam/3rdparty/metis/GKlib"],
+    visibility = ["//visibility:public"],
+)
+
+cc_library(
+    name = "libmetis",
+    srcs = glob(["gtsam/3rdparty/metis/libmetis/*.c"]),
+    hdrs = glob(["gtsam/3rdparty/metis/libmetis/*.h"]),
+    includes = ["gtsam/3rdparty/metis/libmetis"],
+    visibility = ["//visibility:public"],
+    deps = [
+        ":GKlib",
+    ],
+)
+
+cc_library(
+    name = "SuiteSparse_config",
+    srcs = ["gtsam/3rdparty/SuiteSparse_config/SuiteSparse_config.c"],
+    hdrs = ["gtsam/3rdparty/SuiteSparse_config/SuiteSparse_config.h"],
+    includes = ["gtsam/3rdparty/SuiteSparse_config"],
+    visibility = ["//visibility:public"],
+)
+
+gtsam_library(
+    name = "libgtsam",
+    deps = [
+        ":SuiteSparse_config",
+        ":libmetis",
+    ],
+)

......
```
### Handle external packages' dependencies
Refering to `deps` in patch or `BUILD` for external packages
- Use its `WORKSPACE` (if bazel)
```
load("@org_tensorflow//tensorflow:workspace.bzl", "tf_workspace")

tf_workspace(tf_repo_name = "org_tensorflow")
```
- Use external package in current `WORKSPACE`
```
deps = deps + [
    "@com_gitlab_libeigen_eigen//:eigen",
    "@boost//:archive",
    "@boost//:bind",
    "@boost//:variant",
    "@boost//:lexical_cast",
    "@boost//:date_time",
    "@boost//:math",
    "@boost//:serialization",
    "@boost//:core",
    "@boost//:multiprecision",
    "@boost//:filesystem",
],
```
- Use external package in another external package's `WORKSPACE`
```
diff --git a/audio/dsp/spectrogram/BUILD b/audio/dsp/spectrogram/BUILD
index 5c0c45e..b416324 100644
--- a/audio/dsp/spectrogram/BUILD
+++ b/audio/dsp/spectrogram/BUILD
@@ -20,7 +20,7 @@ cc_library(
         "//audio/dsp:number_util",
         "//audio/dsp:porting",
         "//audio/dsp:window_functions",
-        "//third_party/fft2d:fft2d",
-        "@com_github_glog_glog//:glog",
+        "@org_tensorflow//third_party/fft2d:fft2d_headers",
+        "//third_party:glog",
     ],
 )
```

### Writing custom rules
sample customized rule for complicated package
```
def gtsam_library(name, deps = []):
    gtsam = "external/%s" % native.repository_name().lstrip("@")
    srcs = native.glob(["gtsam/**/*.cpp"], exclude = ["gtsam/3rdparty/**/*.cpp", "gtsam/precompiled_header.cpp", "gtsam/**/tests/*"]) + ["gtsam/3rdparty/CCOLAMD/Source/ccolamd.c"]
    hrds = native.glob(["gtsam/**/*.h"], exclude = ["gtsam/base/chartTesting.h", "gtsam/precompiled_header.h", "gtsam/3rdparty/**/*.h"]) + native.glob(["gtsam/3rdparty/ceres/*.h"]) + ["gtsam/3rdparty/metis/include/metis.h"] + ["gtsam/3rdparty/CCOLAMD/Include/ccolamd.h"]
    return native.cc_library(
        name = name,
        srcs = srcs + hrds,
        hdrs = hrds,
        copts = [
            "-I" + gtsam,
        ],
        defines = [
            "GTSAM_ALLOCATOR_STL",
            "GTSAM_THROW_CHEIRALITY_EXCEPTION",
            "GTSAM_ALLOW_DEPRECATED_SINCE_V4",
        ],
        includes = ["."],
        visibility = ["//visibility:public"],
        deps = deps + [
            "@com_gitlab_libeigen_eigen//:eigen",
            "@boost//:archive",
            "@boost//:bind",
            "@boost//:variant",
            "@boost//:lexical_cast",
            "@boost//:date_time",
            "@boost//:math",
            "@boost//:serialization",
            "@boost//:core",
            "@boost//:multiprecision",
            "@boost//:filesystem",
        ],
    )
```
use it by adding `load("//:gtsam.bzl", "gtsam_library")`
