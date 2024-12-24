---
docs:
  - https://prometheus.fuckcloudnative.io/
git: https://github.com/prometheus
link: https://prometheus.io/
aliases:
  - 普罗米修斯
reference links:
  - https://prometheus.fuckcloudnative.io/
---

# 什么是 Prometheus

[Prometheus](https://github.com/prometheus) 前 Google 工程师 从 2012 年开始在 [Soundcloud](http://soundcloud.com/) 以开源软件的形式进行研发的 **系统监控** 和 **告警工具包**

# 优势

- 由 **指标名称** 和 **键/值** 对 **标签标识** 的时间序列数据组成的多维 [数据模型](https://github.com/yangchuansheng/prometheus-handbook/tree/c6e1e12588ec63c20345090368b37654ef30922a/2-concepts/data_model.html)
- - 强大的 [查询语言 PromQL](https://github.com/yangchuansheng/prometheus-handbook/tree/c6e1e12588ec63c20345090368b37654ef30922a/4-prometheus/basics.html)
- 不依赖分布式存储；单个服务节点具有自治能力。
- 时间序列数据是服务端通过 HTTP 协议主动拉取获得的。
- 也可以通过中间网关来 [推送时间序列数据](https://github.com/yangchuansheng/prometheus-handbook/tree/c6e1e12588ec63c20345090368b37654ef30922a/5-instrumenting/pushing.html)。
- 可以通过静态配置文件或服务发现来获取监控目标。
- 支持多种类型的图表和仪表盘。

# Prometheus 的组件

Prometheus 生态系统由多个组件组成，其中有许多组件是可选的：

- [Prometheus Server](https://github.com/prometheus/prometheus) 作为服务端，用来存储时间序列数据。
- [客户端库](https://github.com/yangchuansheng/prometheus-handbook/tree/c6e1e12588ec63c20345090368b37654ef30922a/5-instrumenting/clientlibs.html) 用来检测应用程序代码。
- 用于支持临时任务的 [推送网关](https://github.com/prometheus/pushgateway)。
- [Exporter](https://github.com/yangchuansheng/prometheus-handbook/tree/c6e1e12588ec63c20345090368b37654ef30922a/5-instrumenting/exporters.html) 用来监控 HAProxy，StatsD，Graphite 等特殊的监控目标，并向 Prometheus 提供标准格式的监控样本数据。
- [alartmanager](https://github.com/prometheus/alertmanager) 用来处理告警。
- 其他各种周边工具。

其中大多数组件都是用 [Go](https://golang.org/) 编写的，因此很容易构建和部署为静态二进制文件。

# 架构

![[Pasted image 20240822143937.png]]
Prometheus Server 直接从监控目标中或者间接通过推送网关来拉取监控指标，它在本地存储所有抓取到的样本数据，并对此数据执行一系列规则，以汇总和记录现有数据的新时间序列或生成告警。可以通过 [Grafana](https://grafana.com/) 或者其他工具来实现监控数据的可视化。

# 适用场景

- 记录 **文本格式** 的 **[[300_Software/Prometheus/时间序列]]**
