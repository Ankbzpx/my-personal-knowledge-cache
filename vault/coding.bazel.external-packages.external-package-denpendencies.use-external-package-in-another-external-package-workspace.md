---
id: SIhHkfP5qnnAGZDEAmmZb
title: Use External Package in Another External Package Workspace
desc: ''
updated: 1640016071258
created: 1640016060503
---

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