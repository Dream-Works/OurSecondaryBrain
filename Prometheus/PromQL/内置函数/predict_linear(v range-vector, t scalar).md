---
tags:
  - Prometheus/PromQL/内置函数
---
函数可以预测时间序列 v 在 t 秒后的值。它基于简单线性回归的方式，对时间窗口内的样本数据进行统计，从而可以对时间序列的变化趋势做出预测。该函数的返回结果**不带有度量指标**，只有标签列表。

例如，基于 2 小时的样本数据，来预测主机可用磁盘空间的是否在 4 个小时候被占满，可以使用如下表达式：

```promql
predict_linear(node_filesystem_free{job="node"}[2h], 4 * 3600) < 0
```

通过下面的例子来观察返回值：

```promql
predict_linear(http_requests_total{code="200",instance="120.77.65.193:9090",job="prometheus",method="get"}[5m], 5)
结果：
{code="200",handler="query_range",instance="120.77.65.193:9090",job="prometheus",method="get"}  1
{code="200",handler="prometheus",instance="120.77.65.193:9090",job="prometheus",method="get"}   4283.449995397104
{code="200",handler="static",instance="120.77.65.193:9090",job="prometheus",method="get"}   22.99999999999999
...
```

这个函数一般只用在 Gauge 类型的时间序列上。
