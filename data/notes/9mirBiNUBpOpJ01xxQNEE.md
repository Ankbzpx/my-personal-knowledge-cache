
## Rules
### `http_archive`
`BUILD`, patch

```
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
```

### `git_repository`
commit, patch
```
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")
```
### `new_git_repository`
commit, `BUILD` or patch
```
load("@bazel_tools//tools/build_defs/repo:git.bzl", "new_git_repository")
```
## Native Bazel Support

```
http_archive(
    name = "com_github_gflags_gflags",
    sha256 = "34af2f15cf7367513b352bdcd2493ab14ce43692d2dcd9dfc499492966c64dcf",
    strip_prefix = "gflags-2.2.2",
    urls = ["https://github.com/gflags/gflags/archive/v2.2.2.tar.gz"],
)
```
`strip_prefix` is the name of the root folder of the archive
