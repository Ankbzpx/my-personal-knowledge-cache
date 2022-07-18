---
id: moecnjx53oqjtnw3lzixsfw
title: Basics
desc: ''
updated: 1658037816638
created: 1658024450849
---
> Reference: https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#programming-model

## Concepts

**Kernel**: C++ function executed N times in parallel by N different CUDA threads. 

**Grid of Thread Blocks** (Global memory): `umBlocks` x `threadPerBlock`, both `numBlocks` and `threadPerBlock` can be `int` or `dim3`.

**Thread block** (Per-block shared memory): 1-3D block of threads, indexed by `threadIdx`. Num of threads per block is limited up to 1024. It is required to be executed independently.
```
dim3 threadPerBlock(N) -> N x 1 x 1
dim3 threadPerBlock(N, N) -> N x N x 1
dim3 threadPerBlock(N, N, N) -> N x N x N
```

**Threads within block** (Per-thread local memory): can have shared memory access, can be synced by `__syncthreads()` (like [Fence](https://chromium.googlesource.com/chromium/src/+/HEAD/docs/design/gpu_synchronization.md))

**Host**: CPU and its memory

**Device**: GPU and its memory

## Function Execution Space Specifier
> https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#function-declaration-specifiers

| Specifier  | Executed | Callable      |
|:----------:|:--------:|:-------------:|
|`__global__`| Device   | Host & Device |
|`__device__`| Device   | Device only   |
|`__host__`  | Host     | Host only     |

## TODO
- [x] [function-declaration](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#function-declaration-specifiers)