---
id: 3bYICX0QbkWk6YuuKb4lF
title: Pyenv
desc: ''
updated: 1646102320210
created: 1644581140474
---

## Install

```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

## Setup
> .zprofile

```
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - )"  or eval "$(pyenv init --path)"
```

WARNING: [instruction](https://github.com/pyenv/pyenv) states `eval "$(pyenv init --path)"` but doesn't always work on ArchLinux

> .zshrc

```
eval "$(pyenv init -)"
```

## Build shared library
```
CONFIGURE_OPTS=--enable-shared
```

## Install 3.6 on ArchLinux

```
CFLAGS="-O2"
```
