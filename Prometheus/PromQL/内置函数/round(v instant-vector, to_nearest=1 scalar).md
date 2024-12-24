---
tags:
  - Prometheus/PromQL/内置函数
---
函数与 [[ceil(v instant-vector)|ceil()]] 和 [[floor(v instant-vector)|floor()]] 函数类似，返回向量中所有样本值的最接近的整数。`to_nearest` 参数是可选的,默认为 1,表示样本返回的是最接近 1 的整数倍的值。你也可以将该参数指定为任意值（也可以是小数），表示样本返回的是最接近它的整数倍的值。
