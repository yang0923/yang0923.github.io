---
title: numpy相关
date: 2025-07-03 17:20:26
tags: [numpy]
categories: [python]

---

## 1. 高维ndarray，报错：超出存储，如何解决？

分块处理：将数据分成较小的块来处理，而不是一次性加载整个数据集。这样可以减少对内存的需求。

使用Dask并行计算库

    ```python
    import dask.array as da

    # 创建一个 Dask 数组
    data = da.random.random((5, 50, 300, 100, 30, 49), chunks=(1, 10, 100, 50, 10, 10))

    # 对每个块进行处理
    def process_block(block):
        # 在这里对块进行处理
        print(f"Processing block with shape {block.shape}")

    # 使用 Dask 计算
    data.map_blocks(process_block).compute()

    ```

使用Zarr库

    ```python
    import zarr
    import numpy as np

    # 创建一个 Zarr 数组
    data = zarr.zeros((5, 50, 300, 100, 30, 49), chunks=(1, 10, 100, 50, 10, 10), dtype=np.float64)

    # 对每个块进行处理
    def process_block(block):
        # 在这里对块进行处理
        print(f"Processing block with shape {block.shape}")

    # 使用 Zarr 的块读取
    for block in data.iter_chunks():
        process_block(block)

    ```
