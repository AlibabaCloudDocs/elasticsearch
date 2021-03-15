# 使用bulk聚合插件（faster-bulk）

faster-bulk插件是阿里云Elasticsearch团队自研的插件，目的是为了提高写入吞吐和降低写入拒绝。该插件能够实现将bulk写入请求按照指定bulk请求大小和时间间隔进行批量聚合，防止过小的bulk请求阻塞写入队列，有效提升写入吞吐。本文介绍faster-bulk插件的适用场景和使用方法。

## 适用场景

faster-bulk插件适用于写入吞吐高、索引分片数多的场景，实测对这类场景写入吞吐提升20%以上，能有效降低写入拒绝，测试说明如下。

**说明：** faster-bulk实现的本质是对bulk写入请求进行批量聚合后再写入shard，因此建议不要在写入延时要求较高的场景中使用。

-   测试环境
    -   节点：3 \* 16核64GB数据节点 + 2 \* 16核64GB独立协调节点。
    -   数据集：esrally官方数据集nyc-taxis，单文档大小为650B。
    -   参数：apack.fasterbulk.combine.interval设置为200ms。
    -   translog状态：分别对translog在同步及异步状态进行测试。
-   测试结果

    |translog状态|写入性能（原生集群，未使用faster-bulk插件）|写入性能（阿里云集群，使用faster-bulk插件）|性能提升|
    |----------|---------------------------|---------------------------|----|
    |同步状态|182314/s|226242/s|23%|
    |异步状态|218732/s|241060/s|10%|

-   结论

    由实验数据对比可得，使用faster-bulk插件后，translog同步或异步状态下写入性能均有所提升，同步状态（默认）下写入性能提升了23%。


## 前提条件

您已完成以下操作：

-   创建阿里云Elasticsearch实例，版本为6.7.0或7.10.0。

    具体操作步骤请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

    **说明：** faster-bulk插件目前仅支持阿里云Elasticsearch 6.7.0和7.10.0版本（商业版和增强版）。

-   安装faster-bulk插件。

    具体操作步骤请参见[安装或卸载系统默认插件](/cn.zh-CN/Elasticsearch/插件配置/安装或卸载系统默认插件.md)。插件安装后，bulk聚合功能默认关闭，使用前需要先开启该功能。


## 开启bulk聚合功能

1.  登录阿里云Elasticsearch实例的Kibana控制台。

    具体操作步骤请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**。

3.  在**Console**中，执行以下命令，开启bulk聚合功能。

    ```
    PUT _cluster/settings
    {
       "transient" : {
          "apack.fasterbulk.combine.enabled":"true"
       }
    }
    ```

    **说明：** 您也可以通过[curl命令](/cn.zh-CN/开发指南/通过API操作阿里云Elasticsearch.md)或第三方可视化插件执行以上命令。


## 设置bulk聚合大小和时间间隔

执行以下命令，指定bulk请求的聚合大小和时间间隔。当单个数据节点上，bulk请求的累计大小或聚合时间间隔达到阈值，即会触发数据写入。

```
PUT _cluster/settings
{
   "transient" : {
      "apack.fasterbulk.combine.flush_threshold_size":"1mb",
      "apack.fasterbulk.combine.interval":"50"
   }
}
```

-   apack.fasterbulk.combine.flush\_threshold\_size：聚合的bulk请求的最大值，默认值为1mb。
-   apack.fasterbulk.combine.interval：聚合的bulk请求的最大时间间隔，单位为ms，默认值为50。

**说明：** 对于海量数据高并发场景，在集群可承受的压力范围内，可适当将最大聚合大小或最大时间间隔调大，减少bulk请求阻塞写入队列。

## 关闭bulk聚合功能

执行以下命令，关闭bulk聚合功能。

```
PUT _cluster/settings
{
   "transient" : {
      "apack.fasterbulk.combine.enabled":"false"
   }
}
```

