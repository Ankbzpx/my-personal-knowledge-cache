---
id: 3bYICX0QbkWk6YuuKb4lF
title: Pyenv
desc: ''
updated: 1644581565518
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
eval "$(pyenv init - )"
```

WARNING: [instruction](https://github.com/pyenv/pyenv) states `eval "$(pyenv init --path)"` but didn't workout on ArchLinux

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
