---
aliases:
  - 仪表盘
tags:
  - Prometheus
---
Gauge 类型代表一种样本数据可以任意变化的指标，即可增可减。Gauge 通常用于像温度或内存使用率这种指标数据，也可以表示能随时增加或减少的“总数”，例如：当前并发请求的数量。

对于 Gauge 类型的监控指标，通过 PromQL 内置的函数 [[delta(v range-vector)|delta()]] 可以获取样本在一段事件内的变化情况，例如，计算 CPU 温度在两小时内的差异：
```PromQL
delta(cpu_temp_celsius{host="zeus"}[2h])
```

还可以通过 [[PromQL]] 内置函数 [[predict_linear()]] 基于简单线回归的方式，对样本数据的变化趋势做出预测。例如，基于 2 小时的样本数据，来预测主机可用磁盘空间在 4 个小时之后的剩余情况：
```PromQL
predict_linear(node_filesystem_free{job="node"}[2h], 4 * 3600) < 0
```

## 参考

- [Prometheus 中文文档/概念/指标类型/仪表盘](https://prometheus.fuckcloudnative.io/di-er-zhang-gai-nian/metric_types#gauge-yi-biao-pan)
