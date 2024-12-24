---
tags:
  - Prometheus/PromQL/内置函数
---
为了能够让客户端的图标更具有可读性，可以通过 `label_replace` 函数为时间序列添加额外的标签。label_replace 的具体参数如下：

```
label_replace(v instant-vector, dst_label string, replacement string, src_label string, regex string)
```

该函数会依次对 v 中的每一条时间序列进行处理，通过 `regex` 匹配 src_label 的值，并将匹配部分 `relacement` 写入到 dst_label 标签中。如下所示：

```
label_replace(up, "host", "$1", "instance",  "(.*):.*")
```

函数处理后，时间序列将包含一个 `host` 标签，host 标签的值为 Exporter 实例的 IP 地址：

```
up{host="localhost",instance="localhost:8080",job="cadvisor"}   1
up{host="localhost",instance="localhost:9090",job="prometheus"}   1
up{host="localhost",instance="localhost:9100",job="node"}   1
```
