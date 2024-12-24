---
tags:
  - Prometheus/PromQL/内置函数
---
输入一个瞬时向量和最小值，样本数据值若小于 min，则改为 min，否则不变。

例如：
```promql
node_load5{instance="192.168.1.75:9100"} # 结果为 2.79
clamp_min(node_load5{instance="192.168.1.75:9100"}, 3) # 结果为 3
```
