---
tags:
  - Prometheus/PromQL/内置函数
---
输入一个区间向量， 返回这个区间向量内每个样本数据值变化的次数（瞬时向量）。

例如：
```promql
# 如果样本数据值没有发生变化，则返回结果为 1
changes(node_load5{instance="192.168.1.75:9100"}[1m]) # 结果为 1
```
