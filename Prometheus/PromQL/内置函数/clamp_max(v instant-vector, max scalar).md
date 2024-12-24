---
tags:
  - Prometheus/PromQL/内置函数
---
输入一个瞬时向量和最大值，样本数据值若大于 max，则改为 max，否则不变。

例如：
```promql
node_load5{instance="192.168.1.75:9100"} # 结果为 2.79
clamp_max(node_load5{instance="192.168.1.75:9100"}, 2) # 结果为 2
```
