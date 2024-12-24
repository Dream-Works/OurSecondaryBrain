---
tags:
  - Prometheus/PromQL/内置函数
---
返回各个样本值的 `e` 的指数值，即 e 的 N 次方。当 N 的值足够大时会返回 `+Inf`。特殊情况为：

- `Exp(+Inf) = +Inf`
- `Exp(NaN) = NaN`
