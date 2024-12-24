---
tags:
  - Prometheus/PromQL/内置函数
---
下面的函数列表允许传入一个区间向量，它们会聚合每个时间序列的范围，并返回一个瞬时向量：

- `avg_over_time(range-vector)` : 区间向量内每个度量指标的平均值。
- `min_over_time(range-vector)` : 区间向量内每个度量指标的最小值。
- `max_over_time(range-vector)` : 区间向量内每个度量指标的最大值。
- `sum_over_time(range-vector)` : 区间向量内每个度量指标的求和。
- `count_over_time(range-vector)` : 区间向量内每个度量指标的样本数据个数。
- `quantile_over_time(scalar, range-vector)` : 区间向量内每个度量指标的样本数据值分位数，φ-quantile (0 ≤ φ ≤ 1)。
- `stddev_over_time(range-vector)` : 区间向量内每个度量指标的总体标准差。
- `stdvar_over_time(range-vector)` : 区间向量内每个度量指标的总体标准方差。

> **[info] 注意**
>
> 即使区间向量内的值分布不均匀，它们在聚合时的权重也是相同的。
