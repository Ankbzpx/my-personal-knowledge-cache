---
id: r9dgj13o31gjsb8psav63xa
title: Parallelism
desc: ''
updated: 1646727643705
created: 1646716192129
---

> `thread` alone won't guarantee parallelism due to [GIL](https://wiki.python.org/moin/GlobalInterpreterLock)


## Multiprocessing

> unwrap for nested for loop

```
import multiprocessing


with multiprocessing.Pool(multiprocessing.cpu_count()) as p:
    # func has single argument
    p.map(func, args1)
    # func has multiple arguments
    p.starmap(func, zip(args1, args2))
```


## Joblib

```
pip install joblib
```

```
from joblib import Parallel, delayed
import multiprocessing

Parallel(n_jobs=multiprocessing.cpu_count())(
    delayed(func)(arg1, arg2) for (arg1, arg2) in zip(args1, args2)
```

> Support nested case

```
from joblib import Parallel, delayed
import multiprocessing

Parallel(n_jobs=multiprocessing.cpu_count())(
    delayed(func)(arg1, arg2) for arg1 in args1 for arg2 in args2)
```