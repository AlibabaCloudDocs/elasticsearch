---
keyword: [es 物理复制, es apack]
---

# 使用apack插件的物理复制功能

apack插件是阿里云Elasticsearch团队自研的插件，支持物理复制和向量检索功能，本文仅介绍物理复制功能。物理复制功能适用于索引有副本、写入数据量大、对数据写入后可见性延迟要求不高的场景，例如日志场景、时序分析场景等，可以大幅度降低CPU开销，提升写入性能。

-   创建阿里云Elasticsearch实例，版本为6.7.0且内核版本为1.2.0及以上，或7.10.0。本文以阿里云Elasticsearch 6.7.0为例介绍。创建实例的方法，请参见[t134282.md\#](/intl.zh-CN/Elasticsearch/管理实例/创建阿里云Elasticsearch实例.md)。
-   安装apack插件。

    目前仅6.7.0和7.10.0版本的阿里云Elasticsearch实例支持apack插件。当6.7.0实例的内核版本为1.2.0以下时，需[升级内核版本](/intl.zh-CN/Elasticsearch/版本升级/升级版本.md)后使用该插件；当实例的内核版本为1.2.0及以上时，系统默认已安装apack插件，不可卸载。您可在[插件配置](/intl.zh-CN/Elasticsearch/插件配置/插件配置概述.md)页面查看。

    **说明：** apack插件安装后，您既可以使用物理复制功能，也可以使用向量检索功能。本文仅介绍物理复制功能的使用方法，向量检索功能的使用方法请参见[使用向量检索插件（aliyun-knn）]()。


物理复制功能的基本原理为： 阿里云Elasticsearch中索引的主分片和副本分片（以下简称主副分片）之间的同步原理默认与原生Elasticsearch一样，即请求先写入主分片，再由主分片同步给副本分片，此时主副本分片都会写索引文件及translog。开启物理复制功能后，Elasticsearch主分片写入机制与原生Elasticsearch一样，既写索引文件也写translog，而副本分片只写translog以保证数据的可靠性和一致性。主分片在每次refresh时，通过网络将增量的索引文件拷贝到副本分片，在主副分片分配可见性延迟略增加的情况下，大幅度提高了集群的写入性能。

物理复制功能的性能测试信息如下：

-   测试环境
    -   机器配置：数据节点8核32GB\*5 + 2TB SSD云盘。
    -   数据集：官方Rally自带的nyc\_taxis（74GB）。
    -   索引配置：使用默认配置（5个主分片，1个副本分片）。
-   测试结果

    |产品|写入速度（doc/s）|
    |--|-----------|
    |原生Elasticsearch 6.7.0|127305|
    |阿里云Elasticsearch 6.7.0，并开启物理复制功能|184592|

-   结论

    与原生Elasticsearch相比，阿里云Elasticsearch在开启了物理复制功能后，写入性能提升大于45%。


**说明：** 本文中的命令均可在Kibana控制台中执行，详情请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

## 注意事项

-   apack插件的物理复制功能作用于索引。对于插件安装前创建的索引，默认未开启，使用时需要先开启。对于插件安装后创建的索引，默认开启。
-   阿里云Elasticsearch支持将已开启物理复制功能的索引切回到原生模式（主副分片都会写索引和translog），但需要先关闭该索引。
-   在为原生模式的索引开启物理复制功能前，需要先关闭该索引，并将副本数设置为0。

## 开启物理复制功能

在创建索引时，通过settings开启物理复制功能。

```
PUT index-1
{
"settings": {
 "index.replication.type" : "segment"
 }
}
```

## 关闭物理复制功能

1.  关闭索引。

    ```
    POST index-1/_close
    ```

2.  更新索引settings，关闭物理复制功能。

    ```
    PUT index-1/_settings
    {
    "index.replication.type" : null
    }
    ```

3.  打开索引。

    ```
    POST index-1/_open
    ```


## 为已有索引开启物理复制功能

1.  将索引的副本数设置为0。

    ```
    PUT index-1/_settings
    {
      "index.number_of_replicas": 0
    }
    ```

2.  关闭索引。

    ```
    POST index-1/_close
    ```

3.  更新索引settings，打开物理复制功能。

    ```
    PUT index-1/_settings
    {
    "index.replication.type" : "segment"
    }
    ```

4.  打开索引。

    ```
    POST  index-1/_open
    ```

5.  将索引的副本数设置为1。

    ```
    PUT index-1/_settings
    {
      "index.number_of_replicas": 1
    }
    ```


