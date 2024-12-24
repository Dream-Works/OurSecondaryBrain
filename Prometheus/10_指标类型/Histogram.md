---
aliases:
  - 直方图
tags:
  - Prometheus
---
在大多数情况下人们都倾向于使用某些量化指标的平均值，例如 CPU 的平均使用率、页面的平均响应事件。
这种方式的问题很明显，以系统 API 调用的平均响应时间为例：
如果大多数 API 请求都维持在 100ms 的响应时间范围呢，而个别请求的响应时间需要 5s，那么就会导致某些 WEB 页面的响应时间落到中位数的情况，而这种现象被称为**[[长尾问题]]**

为了区分是平均的慢还是长尾的慢，最简单的方式就是按照请求延迟的范围进行分组。
例如，统计延迟在 0~10ms 之间和 10~20ms 之间的请求数有多少。通过这种方式可以快速分析系统慢的原因。 [[Histogram]] 和 [[Summary]] 都是为了能够解决这样问题的存在，通过 [[Histogram]] 和 [[Summary]] 类型的监控指标，我们可以快速了解监控样本的分布情况。

[[Histogram]] 在一段时间范围内对数据进行采样（通常是请求持续时间或响应大小等），并将其计入可配置的存储同（bucket）中，后续可通过指定区间筛选样本，也可以统计样本总数，最后一般将数据展示为直方图。

[[Histogram]] 类型的样本会提供三种指标（假设指标名称为 `<basename>`）：
- 样本的值分布在 bucket 中的数量，命名为 `<basename>_bucket{le="<上边界>"}`。解释的更通俗易懂一点，这个值指标值 <= 上边界的所有样本数量。
  ```promql
  // 在总共2次请求当中。http 请求响应时间 <=0.005 秒 的请求次数为0  
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.005",} 0.0
  // 在总共2次请求当中。http 请求响应时间 <=0.01 秒 的请求次数为0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.01",} 0.0
  // 在总共2次请求当中。http 请求响应时间 <=0.025 秒 的请求次数为0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.025",} 0.0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.05",} 0.0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.075",} 0.0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.1",} 0.0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.25",} 0.0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.5",} 0.0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="0.75",} 0.0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="1.0",} 0.0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="2.5",} 0.0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="5.0",} 0.0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="7.5",} 2.0
  // 在总共2次请求当中。http 请求响应时间 <=10 秒 的请求次数为 2
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="10.0",} 2.0
  io_namespace_http_requests_latency_seconds_histogram_bucket{path="/",method="GET",code="200",le="+Inf",} 2.0
  ```
- 所有样本值的大小总和，命名为 `<basename>_sum`。
  ```promql
  // 实际含义： 发生的2次 http 请求总的响应时间为 13.107670803000001 秒
  io_namespace_http_requests_latency_seconds_histogram_sum{path="/",method="GET",code="200",} 13.107670803000001
  ```
- 样本总数，命名为 `<basename>_count`。值和 `<basename>_bucket{le="+Inf"}` 相同。
  ```promql
  // 实际含义： 当前一共发生了 2 次 http 请求
  io_namespace_http_requests_latency_seconds_histogram_count{path="/",method="GET",code="200",} 2.0
  ```

> **[info] 注意**
>
> bucket 可以理解为是对数据指标值域的一个划分，划分的依据应该基于数据值的分布。注意后面的采样点是包含前面的采样点的，假设 `xxx_bucket{..., le="0.01"}` 的值为 10， 而 `xxx_bucket{...,le="0.05"}` 的值为 30，那么意味着这 30 个采样点中，有 10 个是小于 10ms 的，其余 20 个采样点的响应时间是介于 10ms 和 50ms 之间的。
>

可以理解为

| 响应时间 | 0~10ms | 10~50ms |
| ---- | ------ | ------- |
| 数量   | 10     | 20      |
可以通过 [[histogram_quantile(φ float, b instant-vector)|histogram_quantile()函数]] 来计算 [[Histogram]] 类型样本的 [[分位数]]。分位数可以理解为分割数据的点。
假设：样本的 9 分为数（quantile=0.9）的值为 x，即表示小于 x 的采样值的数量占总体采样值的 90%。

[[Histogram]] 还可以用来计算应用性能指标值（[Apdex score](https://www.wikiwand.com/en/articles/Apdex)）

## 参考

- [Prometheus 中文文档/概念/指标类型/直方图](https://prometheus.fuckcloudnative.io/di-er-zhang-gai-nian/metric_types#histogram-zhi-fang-tu)
