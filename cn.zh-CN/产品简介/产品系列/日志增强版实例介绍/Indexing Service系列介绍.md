# Indexing Service系列介绍

您可以根据业务的读写需求选择使用阿里云Elasticsearch日志增强版Indexing Service系列，通过其云上写入托管能力，体验按需购买、按量付费的低成本、高性能的时序日志场景下的Elasticsearch服务。本文主要介绍Indexing Service的适用场景、架构、优势以及性能测试结果。

阿里云Elasticsearch支持通用商业版和日志增强版两种类型的实例。其中，阿里云Elasticsearch团队推出基于读写分离架构的日志增强版Indexing Service。Indexing Service不仅实现了Elasticsearch集群的云端写入Indexing Service托管，还从硬件选型、集群架构、内核性能进行了全方位优化。在提升集群写入性能的同时，您可以从读写角度分别评估业务需求，根据实际写入按量付费，而无须按照集群峰值写入吞吐预留资源，大大降低了云上使用Elasticsearch的资源成本和运维成本。

## 适用场景

适用于写入TPS较高、写入流量波动较大和搜索QPS较低的时序数据分析场景，例如日志检索分析、Metric监控分析、IoT智能硬件数据收集及监控分析等。

## 架构

![fig_01](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8423488161/p265896.png)

此架构具备如下优势：

-   高性能：专家级写入优化，Indexing Service服务通过索引物理复制、计算存储分离、faster-bulk等阿里云自研特性优化写入性能，您无需做任何配置变更即可享受专家级写入性能。
-   低延迟：跨集群实时物理复制，通过segment级别的实时物理复制，在写入流量饱和的情况下，用户集群相对于Indexing Service集群的平均数据延迟达到百毫秒级。
-   高稳定：异地容灾，Indexing Service具备异地多集群备份能力。当某一个集群出现异常时，可切换用户集群的索引托管至另一个正常集群，进一步提升写入高可用性。

## 优势

-   低成本：写入计算资源成本平均降低了60% 。
-   弹性扩展：写入资源由云端Indexing Service后台调配和管理，以应对写入流量波动。在无需数据迁移的情况下，实现日志场景下Elasticsearch集群极致的写入弹性扩展能力，轻松应对高峰流量。
-   免运维：用户无须关注Elasticsearch集群的写入资源和写入压力，由Indexing Service实现云上写入托管，极大降低集群运维成本。

## 使用限制

云端托管功能可以为您创建的Elasticsearch集群提供写入Serverless服务，但是在使用时，对数据写入和索引配置有相关限制。详情请参见下表。

|分类|限制项|限制说明|备注|
|--|---|----|--|
|实例维度|写入流量保护|写入流量最大为200 MB/s|如果超过最大限制，返回状态码429，提示Inflow Quota Exceed。如果您有更大的使用需求，请[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex?accounttraceid=f7b76db740fa486baa4b63bd5848fbc1idrb)申请。|
|写入次数保护|写入次数最大为600000次/秒|如果超过最大限制，返回状态码429，提示Write QPS Exceed。如果您有更大的使用需求，请[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex?accounttraceid=f7b76db740fa486baa4b63bd5848fbc1idrb)申请。|
|Shard维度|写入流量|写入流量最大为5 MB/s|非硬性限制，如果超过最大限制，系统会尽可能服务，但不能保证服务质量。|
|写入次数|写入次数最大为1000次/秒|非硬性限制，如果超过最大限制，系统会尽可能服务，但不能保证服务质量。|
|Shard数|单个索引最多可创建的Shard数|最多可创建300个Shard。|无。|
|配置|index.refresh\_interval|云端托管集群中默认配置此参数，用户侧配置不生效。|无。|
|index.translog.durability|translog在云端托管集群中默认配置为异步写入模式（index.translog.durability=async），用户侧配置不生效。|无。|
|Ingest NodIe|-   概念

预处理操作允许在索引文档之前，即写入数据之前，通过事先定义好的一系列的processors（处理器）和pipeline（管道），对数据进行某种转化和富化。详情请参见[Ingest NodIe](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/ingest.html)。

-   使用建议

当您使用时序写入Serverless服务时，如果在indexing之前使用ingest node来预处理documents，则ingest将在用户集群做处理。建议不要使用过于复杂的加工处理逻辑。 |

## 性能测试

-   测试环境：
    -   数据集：官方[esrally](https://github.com/elastic/rally/tree/master/esrally)提供的[nyc\_taxis](https://github.com/elastic/rally-tracks/tree/master/nyc_taxis)。
    -   集群规格：分别使用3个数据节点的2核8 GB、4核16 GB和8核32 GB规格。
    -   索引配置：分片数为15，副本数为1，均开启translog异步写入和物理复制功能，refresh\_interval为5秒。
-   测试结果：

    |规格|实例版本|写入TPS|写入可见性延迟|
    |--|----|-----|-------|
    |3个数据节点的2核8 GB|通用商业版|24883|5秒|
    |日志增强版Indexing Service|226649|6秒|
    |3个数据节点的4核16 GB|通用商业版|52372|5秒|
    |日志增强版Indexing Service|419574|6秒|
    |3个数据节点的8核32 GB|通用商业版|110277|5秒|
    |日志增强版Indexing Service|804010|6秒|

-   测试结论：

    日志增强版Indexing Service与通用商业版性能对比结果：

    -   基于3个数据节点的2核8 GB规格，性能提升了910%。
    -   基于3个数据节点的4核16 GB规格，性能提升了801%。
    -   基于3个数据节点的8核32 GB规格，性能提升了729%。

## 相关文档

-   [查看集群状态和节点信息](/cn.zh-CN/Elasticsearch/实例管理/查看集群状态和节点信息.md)
-   [数据流管理](/cn.zh-CN/Elasticsearch/索引管理中心/数据流管理.md)
-   [索引管理](/cn.zh-CN/Elasticsearch/索引管理中心/索引管理.md)
-   [索引模板管理](/cn.zh-CN/Elasticsearch/索引管理中心/索引模板管理.md)
-   [Indexing Service系列介绍](/cn.zh-CN/产品简介/产品系列/日志增强版实例介绍/Indexing Service系列介绍.md)
-   [基于Indexing Service实现数据流管理](/cn.zh-CN/最佳实践/基于Indexing Service实现数据流管理.md)

