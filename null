# Indexing Service

您可以根据业务的读写需求选择使用阿里云Elasticsearch日志增强版Indexing Service系列，通过其云上写入托管能力，体验按需购买、按量付费的低成本、高性能的时序日志场景下的Elasticsearch服务。本文主要介绍Indexing Service的适用场景、架构、优势以及性能测试结果。

阿里云Elasticsearch支持通用商业版和日志增强版两种类型的实例。其中，阿里云Elasticsearch团队推出基于读写分离架构的日志增强版Indexing Service。Indexing Service不仅实现了Elasticsearch集群的云端写入Servcerless托管，还从硬件选型、集群架构、内核性能进行了全方位优化。在提升集群写入性能的同时，还可以使您能够根据实际写入按量付费，而无须按照集群峰值写入吞吐预留资源，大大降低了云上使用Elasticsearch的资源成本和运维成本。

## 适用场景

适用于写入TPS较高、写入流量波动较大和搜索QPS较低的时序数据分析场景，例如日志检索分析、Metric监控分析、IoT智能硬件数据收集及监控分析等。

## 架构

![fig_01](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8423488161/p265896.png)

此架构具备如下能力：

-   跨集群实时物理复制，通过segment级别的实时物理复制，在写入流量饱和的情况下，用户集群相对于Indexing Service集群的平均数据延迟达到百毫秒级。
-   异地容灾，Indexing Service具备异地多集群备份能力。当某一个集群出现异常时，可切换用户集群的索引托管至另一个正常集群，进一步提升写入高可用性。

## 优势

-   低成本：写入计算资源成本平均降低了60% 。
-   弹性扩展：写入资源由云端Serverless后台调配和管理，以应对写入流量波动。在无需数据迁移的情况下，实现日志场景下Elasticsearch集群极致的写入弹性扩展能力，轻松应对高峰流量。
-   免运维：用户无须关注Elasticsearch集群的写入资源和写入压力，由Indexing Service实现云上写入托管，极大降低集群运维成本。

## 性能测试

-   测试环境：
    -   数据集：官方[esrally](https://github.com/elastic/rally/tree/master/esrally)提供的[nyc\_taxis](https://github.com/elastic/rally-tracks/tree/master/nyc_taxis)。
    -   集群规格：分别使用3个数据节点的2核8 GB、4核16 GB和8核32 GB规格。
    -   索引配置：分片数为15，副本数为1，均开启translog异步写入和物理复制功能，refresh\_interval为5秒。
-   测试结果：

    |规格|实例版本|写入TPS|写入可见性延迟|
    |--|----|-----|-------|
    |3个数据节点的2核8 GB|通用商业版|24883|5秒|
    |日志增强版Serverless|226649|6秒|
    |3个数据节点的4核16 GB|通用商业版|52372|5秒|
    |日志增强版Serverless|419574|6秒|
    |3个数据节点的8核32 GB|通用商业版|110277|5秒|
    |日志增强版Serverless|804010|6秒|

-   测试结论：

    日志增强版Serverless与通用商业版性能对比结果：

    -   基于3个数据节点的2核8 GB规格，性能提升了910%。
    -   基于3个数据节点的4核16 GB规格，性能提升了801%。
    -   基于3个数据节点的8核32 GB规格，性能提升了729%。

