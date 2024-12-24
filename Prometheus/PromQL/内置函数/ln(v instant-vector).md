---
tags:
  - Prometheus/PromQL/内置函数
---
计算瞬时向量 v 中所有样本数据的自然对数。特殊情况：

- `ln(+Inf) = +Inf`
- `ln(0) = -Inf`
- `ln(x < 0) = NaN`
- `ln(NaN) = NaN`
