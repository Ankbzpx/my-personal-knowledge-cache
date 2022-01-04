---
id: j0YVBI92LvZhfl6C2NGeC
title: External Package Denpendencies
desc: ''
updated: 1641266498881
created: 1641262973692
---

## Use External Package in Current Workspace
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

## Use External Package in Another External Package Workspace
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


## Use External Package Workspace
```
load("@org_tensorflow//tensorflow:workspace.bzl", "tf_workspace")
tf_workspace(tf_repo_name = "org_tensorflow")
```
