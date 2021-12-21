---
id: cUJ7H7SnwdvvzhyH8eByD
title: Use Build File
desc: ''
updated: 1640015652826
created: 1640015435648
---
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
## `Build ` example

> third_party/acl.BUILD

```
cc_library(
    name = "acl",
    hdrs = glob(["includes/**/*.h"]),
    srcs = glob(["includes/**/*.h"]),
    includes = ["includes"],
    visibility = ["//visibility:public"],
)
```
reference it by `"@com_github_nfrechette_acl//:acl"`, or combine it with its dependencies


> third_party/BUILD

```
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
