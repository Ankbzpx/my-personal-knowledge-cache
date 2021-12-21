---
id: 9mirBiNUBpOpJ01xxQNEE
title: External Packages
desc: ''
updated: 1640015242629
created: 1640014991815
---

## `http_archive`
`BUILD`, patch

```
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
```

## `git_repository`
commit, patch
```
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")
```
## `new_git_repository`
commit, `BUILD` or patch
```
load("@bazel_tools//tools/build_defs/repo:git.bzl", "new_git_repository")
```
