---
aliases:
  - 计数器
---
Counter 类型代表一种样本数据单调递增的指标，即只增不减，除非监控系统发生了重置。
例如，可以使用 counter 类型的指标来表示服务的请求数、已完成的任务数、错误发生的次数等。

counter 主要有两个方法：
```PromQl
// 将 counter 值加1
Inc()
// 将指定值加到counter值上，如果指定值 < 0 会 panic.
Add(float64)
```

Counter 类型数据可以让用户方便的了解事件产生的速率的变化，在 [[PromQL]] 内置的相关操作可以提供相应的分析，比如以 HTTP 应用请求量来进行说明：
```PromQL
// 通过rate()函数获取 HTTP 请求量的增长率
rate(http_requests_total[5m])
// 查询当前系统中， 访问量前10的HTTP地址
topk(10, http_requests_total)
```

不要将 counter 类型应用于样本数据**非单调递增**的指标，例如： 当前运行的进程数量（应该用 [[Gauge]] 类型）

## 参考

- [Prometheus 中文文档/概念/指标类型/计数器](https://prometheus.fuckcloudnative.io/di-er-zhang-gai-nian/metric_types#counter-ji-shu-qi)
