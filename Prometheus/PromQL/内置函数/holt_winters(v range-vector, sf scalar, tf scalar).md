---
tags:
  - Prometheus/PromQL/内置函数
---
函数基于区间向量 v，生成时间序列数据平滑值。平滑因子 `sf` 越低, 对旧数据的重视程度越高。趋势因子 `tf` 越高，对数据的趋势的考虑就越多。其中，`0< sf, tf <=1`。

holt_winters 仅适用于 Gauge 类型的时间序列。
