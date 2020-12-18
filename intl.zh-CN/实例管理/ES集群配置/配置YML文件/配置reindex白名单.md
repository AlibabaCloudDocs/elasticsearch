---
keyword: [es reindex, 重建索引]
---

# 配置reindex白名单

通过当前Elasticsearch集群调用reindex API，从远程集群迁移索引数据前，需要先配置reindex白名单。本文为您介绍如何配置reindex白名单。

## 配置说明

参见[配置YML参数](/intl.zh-CN/实例管理/ES集群配置/修改YML参数配置.md)，在**其他Configure配置**中，通过`reindex.remote.whitelist`属性，将远程Elasticsearch集群的访问地址添加到当前集群的远程访问白名单中。

白名单允许以`host`和`port`组合，并使用逗号分隔多个主机配置（例如`otherhost:9200,another:9200,127.0.10.**:9200,localhost:**`）。且白名单不识别协议信息，只使用主机和端口信息用于实现安全策略设定。

配置reindex白名单时，如果远程Elasticsearch集群为单可用区的阿里云Elasticsearch实例，请使用`<阿里云Elasticsearch实例的域名>:9200`；如果为多可用区的阿里云Elasticsearch实例，请使用实例中所有数据节点的IP地址与端口的组合。具体示例如下：

-   单可用区

    ![单可用区配置示例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7202659951/p135190.png)

    ```
    reindex.remote.whitelist: ["es-cn-09k1rgid9000g****.elasticsearch.aliyuncs.com:9200"]
    ```

-   多可用区

    ![多可用区reindex白名单配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6746359951/p135207.png)

    ```
    reindex.remote.whitelist: ["10.0.xx.xx:9200","10.0.xx.xx:9200","10.0.xx.xx:9200","10.15.xx.xx:9200","10.15.xx.xx:9200","10.15.xx.xx:9200"]
    ```

    **说明：** reindex白名单配置完成后，即可调用reindex API重建索引，详情请参见[通过reindex迁移数据](/intl.zh-CN/最佳实践/Elasticsearch迁移/阿里云ES间数据迁移/通过reindex迁移数据.md)。


