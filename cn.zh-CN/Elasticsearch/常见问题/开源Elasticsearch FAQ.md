# 开源Elasticsearch FAQ

本文汇总了开源Elasticsearch相关的常见问题。

## 如何配置索引线程池大小？

在YML参数配置中，指定thread\_pool.write.queue\_size参数的大小即可。具体操作步骤，请参见[配置YML参数](/cn.zh-CN/Elasticsearch/集群配置/配置YML参数.md)。

![配置线程池大小](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5850005061/p180387.png)

**说明：** 对于6.x以下版本的Elasticsearch集群，需要使用thread\_pool.index.queue\_size参数。

## 出现内存溢出OOM（OutOfMemory）的错误，如何处理？

通过以下命令清理缓存，然后观察具体原因，根据原因[升配集群](/cn.zh-CN/Elasticsearch/升降配实例/升配集群.md)或调整业务。

```
curl -u elastic:passwd -XPOST "localhost:9200/<index-name>/_cache/clear?pretty"
```

## 如何手动对shard进行操作？

使用reroute API，或通过Cerebro进行操作。具体操作步骤，请参见[Cluster reroute API](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/cluster-reroute.html)和[Cerebro](/cn.zh-CN/最佳实践/Elasticsearch应用/集群管理/通过Cerebro访问阿里云ES.md)。

## Elasticsearch的缓存清除策略有哪些？

Elasticsearch支持以下三种缓存清除策略：

-   清除全部缓存

    ```
    curl localhost:9200/_cache/clear?pretty
    ```

-   清除单一索引缓存

    ```
    curl localhost:9200/<index_name>/_cache/clear?pretty
    ```

-   清除多索引缓存

    ```
    curl localhost:9200/<index_name1>,<index_name2>,<index_name3>/_cache/clear?pretty
    ```


## 如何重新分配索引分片（reroute）？

当出现分片丢失、分片错误等分片问题时，您可以执行以下命令进行reroute操作。

```
curl -XPOST 'localhost:9200/_cluster/reroute' -d '{
    "commands" : [ {
        "move" :
            {
              "index" : "test", "shard" : 0,
              "from_node" : "node1", "to_node" : "node2"
            }
        },
        {
          "allocate" : {
              "index" : "test", "shard" : 1, "node" : "node3"
          }
        }
    ]
}'
```

## 索引查询时，提示`statusCode: 500`的错误，如何处理？

建议您通过第三方插件进行查询（例如[Cerebro](/cn.zh-CN/最佳实践/Elasticsearch应用/集群管理/通过Cerebro访问阿里云ES.md)）：

-   查询正常：说明该错误大概率是由于索引名称不规范引起的。规范的索引名称只包含英文、下划线和数字，您可以通过修改索引名称来修复此问题。
-   查询不正常：说明索引或集群本身存在问题。请确保集群中存在该索引，且集群处于正常状态。

## 如何修改自动创建索引`auto_create_index`参数？

执行以下命令修改。

```
PUT /_cluster/settings
{
    "persistent" : {
        "action": {
          "auto_create_index": "false"
        }
    }
}
```

**说明：** `auto_create_index`参数的默认值为false，表示不允许自动创建索引。一般建议您不要调整该值，会引起索引太多、索引Mapping和Setting不符合预期等问题。

## OSS快照大概需要多久？

在集群的分片数、内存、磁盘和CPU等正常的情况下，80GB的索引数据进行OSS快照，大约需要30分钟。

## 创建索引时，如何设置分片数？

建议您将单个分片存储索引数据的大小控制在30GB以内，不要超过50GB，否则会极大降低查询性能。建议：最终分片数量 = 数据总量/30GB。

适当提升分片数量可以提升建立索引的速度。分片数过多或过少，都会降低查询速度，具体说明如下：

-   分片数过多会导致需要打开的文件比较多。由于分片是存储在不同机器上的，因此分片数越多，各个节点之间的交互也就越多，导致查询效率降低。
-   分片数过少会导致单个分片索引过大，降低整体的查询效率。

## 自建Elasticsearch迁移数据，使用elasticsearch-repository-oss插件遇到如下问题，如何解决？

问题：`ERROR: This plugin was built with an older plugin structure. Contact the plugin author to remove the intermediate "elasticsearch" directory within the plugin zip`。

解决方案：将elasticsearch改名为elasticsearch-repository-oss， 然后复制到plugins目录下。

## Elasticsearch服务器的时间不准，如何调整？

您可以在Kibana中，通过转换时区来调整服务器时间，如下图（以6.7.0版本为例）。

![调整服务器时间1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5850005061/p180469.png)

![选择时区](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5850005061/p180471.png)

## Elasticsearch的Term查询适用于哪种类型的数据？

Term为单词级别的查询，这些查询通常用于结构化的数据，例如number、date、keyword等，而不是text。

**说明：** 全⽂文本查询之前要先对文本内容进行分词，而单词级别的查询直接在相应字段的反向索引中精确查找，单词级别的查询一般用于数值、日期等类型的字段上。

## 使用Elasticsearch的别名（aliases）功能需要注意哪些问题？

需要将别名里索引的分片控制在1024个以内。

## 在进行查询过程中，出现以下报错，如何处理？

报错：`"type": "too_many_buckets_exception", "reason": "Trying to create too many buckets. Must be less than or equal to: [10000] but was [10001]`

问题分析与解决方案：请参见[控制聚合中创建的桶数](https://xbuba.com/questions/57393548)。除了调整业务聚合的size大小，您还可以参见[Increasing max\_buckets for specific Visualizations](https://discuss.elastic.co/t/increasing-max-buckets-for-specific-visualizations/187390)来处理。

## 如何批量删除索引？

默认情况下，Elasticsearch不允许批量删除索引，需要通过以下命令手动开启。开启后，您可以通过通配符进行批量删除操作。

```
PUT /_cluster/settings
{
  "persistent": {
     "action.destructive_requires_name": false
  }
}
```

