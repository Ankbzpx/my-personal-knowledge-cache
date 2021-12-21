---
id: a4ZF1VGrgs09TjYptn9Uy
title: Use External Package Workspace
desc: ''
updated: 1640015996645
created: 1640015975253
---
```
load("@org_tensorflow//tensorflow:workspace.bzl", "tf_workspace")
tf_workspace(tf_repo_name = "org_tensorflow")
```
