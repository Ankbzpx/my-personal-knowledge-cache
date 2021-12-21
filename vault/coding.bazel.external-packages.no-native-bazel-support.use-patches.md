---
id: VzXeNs7joAXDsA5hn9iSB
title: Use Patches
desc: ''
updated: 1640015732830
created: 1640015674788
---

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
## Patch example

> generated using git diff

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