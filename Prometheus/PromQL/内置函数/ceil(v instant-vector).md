---
tags:
  - Prometheus/PromQL/内置函数
---
将 v 中所有元素的样本值向上四舍五入到最接近的整数。

例如：
```promql
node_load5{instance="192.168.1.75:9100"} # 结果为 2.79
ceil(node_load5{instance="192.168.1.75:9100"}) # 结果为 3
```
