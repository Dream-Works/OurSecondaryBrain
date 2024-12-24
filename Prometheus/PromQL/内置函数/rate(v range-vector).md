---
tags:
  - Prometheus/PromQL/内置函数
---
函数可以直接计算区间向量 v 在时间窗口内平均增长速率，它会在单调性发生变化时 (如由于采样目标重启引起的计数器复位) 自动中断。该函数的返回结果**不带有度量指标**，只有标签列表。

例如，以下表达式返回区间向量中每个时间序列过去 5 分钟内 HTTP 请求数的每秒增长率：

```promql
rate(http_requests_total[5m])
结果：
{code="200",handler="label_values",instance="120.77.65.193:9090",job="prometheus",method="get"} 0
{code="200",handler="query_range",instance="120.77.65.193:9090",job="prometheus",method="get"}  0
{code="200",handler="prometheus",instance="120.77.65.193:9090",job="prometheus",method="get"}   0.2
...
```

rate() 函数返回值类型只能用计数器，在长期趋势分析或者告警中推荐使用这个函数。

> **[info] 注意**
>
> 当将 `rate()` 函数与 [聚合运算符](https://prometheus.io/docs/prometheus/latest/querying/operators/#aggregation-operators)（例如 `sum()`）或随时间聚合的函数（任何以 `_over_time` 结尾的函数）一起使用时，必须先执行 rate 函数，然后再进行聚合操作，否则当采样目标重新启动时 ra
