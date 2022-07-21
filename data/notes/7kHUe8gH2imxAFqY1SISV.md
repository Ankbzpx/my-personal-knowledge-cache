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
