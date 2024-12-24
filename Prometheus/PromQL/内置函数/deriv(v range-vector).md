---
tags:
  - Prometheus/PromQL/内置函数
---
返回一个瞬时向量。它使用 [简单的线性回归](http://en.wikipedia.org/wiki/Simple_linear_regression) 计算区间向量 v 中各个时间序列的导数。

这个函数一般只用在 Gauge 类型的时间序列上。
