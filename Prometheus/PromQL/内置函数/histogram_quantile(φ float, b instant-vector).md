---
tags:
  - Prometheus/PromQL/内置函数
---
`histogram_quantile(φ float, b instant-vector)` 从 bucket 类型的向量 `b` 中计算 φ (0 ≤ φ ≤ 1) 分位数（百分位数的一般形式）的样本的最大值。（有关 φ 分位数的详细说明以及直方图指标类型的使用，请参阅 [直方图和摘要](https://prometheus.io/docs/practices/histograms)）。向量 `b` 中的样本是每个 bucket 的采样点数量。每个样本的 labels 中必须要有 `le` 这个 label 来表示每个 bucket 的上边界，没有 `le` 标签的样本会被忽略。直方图指标类型自动提供带有 `_bucket` 后缀和相应标签的时间序列。

可以使用 `rate()` 函数来指定分位数计算的时间窗口。

例如，一个直方图指标名称为 `employee_age_bucket_bucket`，要计算过去 10 分钟内 第 90 个百分位数
请使用以下表达式：
```
histogram_quantile(0.9, rate(employee_age_bucket_bucket[10m]))
```

返回：
```
{instance="10.0.86.71:8080",job="prometheus"} 35.714285714285715
```

这表示最近 10 分钟之内 90% 的样本的最大值为 35.714285714285715。

这个计算结果是每组标签组合成一个时间序列。我们可能不会对所有这些维度（如 `job`、`instance` 和 `method`）感兴趣，并希望将其中的一些维度进行聚合，则可以使用 sum() 函数。

例如，以下表达式根据 `job` 标签来对第 90 个百分位数进行聚合：
```
# histogram_quantile() 函数必须包含 le 标签
histogram_quantile(0.9, sum(rate(employee_age_bucket_bucket[10m])) by (job, le))
```

如果要聚合所有的标签，则使用如下表达式：
```
histogram_quantile(0.9,sum(rate(employee_age_bucket_bucket[10m])) by (le))
```

> **[info] 注意**
>
> `histogram_quantile` 这个函数是根据假定每个区间内的样本分布是线性分布来计算结果值的 (也就是说它的结果未必准确)，最高的 bucket 必须是 le="+Inf" (否则就返回 NaN)。
>
> 如果分位数位于最高的 bucket（+Inf） 中，则返回第二个最高的 bucket 的上边界。如果该 bucket 的上边界大于 0，则假设最低的 bucket 的的下边界为 0，这种情况下在该 bucket 内使用常规的线性插值。
>
> 如果分位数位于最低的 bucket 中，则返回最低 bucket 的上边界。

如果 b 含有少于 2 个 buckets，那么会返回 `NaN`，如果 φ < 0 会返回 `-Inf`，如果 φ > 1 会返回 `+Inf`。
