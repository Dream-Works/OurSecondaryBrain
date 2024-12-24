---
aliases:
  - 摘要
tags:
  - PromQL
  - Prometheus
---
与 [[Histogram]] 类型类似，用于表示一段时间内的数据采样结果（通常是请求持续时间或响应大小等），但它直接存储了 [[分位数]]（通过客户端计算，然后展示出来），而不是通过区间来计算。

Summary 类型的样本也会提供三种指标 (假设指标名称为)：
- 样本值的分位数分布情况，命名为 `<basename>{quantile="<φ>"}`。
  ```promql
  // 含义：这 12 次 http 请求中有 50% 的请求响应时间是 3.052404983s
  io_namespace_http_requests_latency_seconds_summary{path="/",method="GET",code="200",quantile="0.5",} 3.052404983
  // 含义：这 12 次 http 请求中有 90% 的请求响应时间是 8.003261666s
  io_namespace_http_requests_latency_seconds_summary{path="/",method="GET",code="200",quantile="0.9",} 8.003261666
  ```
- 所有样本值的大小总和，命名为 `<basename>_sum`。
  ```promql
  // 含义：这12次 http 请求的总响应时间为 51.029495508s
  io_namespace_http_requests_latency_seconds_summary_sum{path="/",method="GET",code="200",} 51.029495508
  ```
- 样本总数，命名为 `<basename>_count`。
  ```
  // 含义：当前一共发生了 12 次 http 请求
  io_namespace_http_requests_latency_seconds_summary_count{path="/",method="GET",code="200",} 12.0
  ```

## [[Histogram]] 与 [[Summary]] 的异同

- 他们都包含了 `<basename>_sum` 和 `<basename>_count` 指标
- [[Histogram]] 需要通过 `<basename>_bucket` 来计算 [[分位数]]，而 [[Summary]] 则直接存储了 [[分位数]] 的值。

## 参考

- [Prometheus 中文文档/概念/指标类型/摘要](https://prometheus.fuckcloudnative.io/di-er-zhang-gai-nian/metric_types#summary-zhai-yao)
- [如何区分prometheus中Histogram和Summary类型的metrics？](https://www.cnblogs.com/aguncn/p/9920545.html)
