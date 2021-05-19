---
keyword: [es索引压缩, codec-compression, brotli, zstd]
---

# 使用索引压缩插件beta版本（codec-compression）

codec-compression是阿里云Elasticsearch（简称ES）团队自主开发的索引压缩插件，支持brotli和zstd压缩算法，具有更高的索引压缩比，可以大幅降低索引的存储成本。

您已完成以下操作：

-   创建阿里云ES实例（6.7.0版本），详情请参见[t134282.md\#](/intl.zh-CN/Elasticsearch/管理实例/创建阿里云Elasticsearch实例.md)。

    **说明：** codec-compression插件目前只支持阿里云ES 6.7.0版本，因此请创建6.7.0版本的实例。

-   安装codec-compression插件（新购实例默认已安装）。

    您可在**插件配置**页面查看是否已安装codec-compression插件，如果还未安装，请手动进行安装，安装方法请参见[安装或卸载系统默认插件](/intl.zh-CN/Elasticsearch/插件配置/安装或卸载系统默认插件.md)。


codec-compression插件支持brotli和zstd压缩算法，适用于写入量大、索引存储成本高的场景，例如日志场景、时序分析场景等，可以大大降低索引的存储成本。性能测试信息如下：

-   测试环境
    -   机器配置：数据节点16核64 GB\*3 + 2 TB SSD云盘。
    -   数据集：官方[esrally](https://github.com/elastic/rally/tree/master/esrally)自带的nyc\_taxis（74 GB）。
    -   索引配置：都使用默认配置，写入完成后都可以进行[force merge](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-forcemerge.html#indices-forcemerge)。
-   测试结果

    |压缩算法|索引大小（GB）|写入TPS（doc/s）|
    |----|--------|------------|
    |ES默认压缩算法（LZ4）|35.5|202682|
    |best\_compression（DEFLATE）|26.4|181686|
    |brotli|24.4|182593|
    |zstd|24.6|181393|

-   结论

    codec-compression插件使用的brotli和zstd压缩算法，与默认压缩算法（LZ4）相比，写入性能下降了10%，压缩率提升了45%；与原生best\_compression（DEFLATE）相比，写入性能不变，压缩率提升了8%。


## 操作步骤

1.  登录Kibana控制台。

    登录控制台的具体步骤请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**（开发工具）。

3.  在**Console**中，分别执行如下命令，为索引指定不同的压缩算法。

    -   brotli压缩算法

        ```
        PUT index-1
        {
            "settings": {
                "index": {
                    "codec": "brotli"
                }
            }
        }
        ```

    -   zstd压缩算法

        ```
        PUT index-1
        {
            "settings": {
                "index": {
                    "codec": "zstd"
                }
            }
        }
        ```


