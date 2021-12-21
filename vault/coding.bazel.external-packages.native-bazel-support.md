---
id: 6y5olMeFMhV4BWc4Qb8jA
title: Native Bazel Support
desc: ''
updated: 1640015292367
created: 1640015278803
---

```
http_archive(
    name = "com_github_gflags_gflags",
    sha256 = "34af2f15cf7367513b352bdcd2493ab14ce43692d2dcd9dfc499492966c64dcf",
    strip_prefix = "gflags-2.2.2",
    urls = ["https://github.com/gflags/gflags/archive/v2.2.2.tar.gz"],
)
```
`strip_prefix` is the name of the root folder of the archive
