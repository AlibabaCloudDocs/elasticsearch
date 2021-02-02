# 通过reindex迁移数据

当您需要将旧版本Elasticsearch集群中的数据迁移到当前版本中，或者在本地集群中将数据从旧index复制到新index时，可通过reindex API重建索引来实现。本文介绍具体的实现方法。

## 注意事项

因阿里云Elasticsearch网络架构调整，2020年10月起创建的实例暂不支持Watcher报警和LDAP认证功能，且不支持与2020年10月前创建的实例进行跨集群Reindex、跨集群搜索、跨集群复制等相关操作。即10月前创建的集群，仅支持与10月前创建的集群进行这些操作；10月后创建的集群仅支持与10月后创建的集群进行这些操作。因网络调整带来的影响，待后期功能上线将会解决，请耐心等待。

## 前提条件

-   准备两个阿里云Elasticsearch集群，一个为本地集群，一个为远程集群。

    具体操作，请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/Elasticsearch/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。本地集群和远程集群需要在同一专有网络和虚拟交换机下。本文使用6.7.0版本的实例作为本地集群，6.3.2版本的实例作为远程集群。

-   准备测试数据。
    -   本地集群

        在本地集群中创建目标索引。

        ```
        PUT dest
        {
          "settings": {
            "number_of_shards": 5,
            "number_of_replicas": 1
          }
        }
        ```

    -   远程集群

        在远程集群中准备待迁移的数据。本文使用快速入门章节中的数据测试，详细信息请参见[创建索引](/intl.zh-CN/Elasticsearch/快速入门/业务查询/创建索引.md)和[创建文档并插入数据](/intl.zh-CN/Elasticsearch/快速入门/业务查询/创建文档并插入数据.md)。

        ![本地集群测试数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7202659951/p135747.png)

        **说明：** 如果您使用的是7.0及以上版本的集群，需要将索引类型修改为\_doc。


## 背景信息

reindex的应用场景如下：

-   Elasticsearch集群间迁移数据。
-   索引分片分配不合理，例如数据量太大分片数太少，导致数据导入慢，此时可通过reindex重建索引。
-   索引中存在大量数据的情况下，需要修改索引mapping，重新导入数据到新的索引会比较耗时。此时可通过reindex复制索引数据。

    **说明：** 在Elasticsearch中，定义了索引mapping且导入数据后，将不能再修改索引mapping。


## 操作步骤

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  在顶部菜单栏处，选择资源组和地域，然后在**实例列表**中单击目标实例ID。

4.  在本地集群中，配置reindex白名单。

    1.  在左侧导航栏，单击**ES集群配置**。

    2.  在**YML文件配置**右侧，单击**修改配置**。

    3.  在**YML文件配置**页面的**其他Configure配置**中，配置reindex白名单。

        reindex白名单的配置说明，请参见[配置reindex白名单](/intl.zh-CN/Elasticsearch/ES集群配置/配置YML参数.md)。

        -   对于单可用区实例，白名单的格式为<阿里云Elasticsearch实例的域名\>:9200。

            ![单可用区配置示例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7202659951/p135190.png)

            ```
            reindex.remote.whitelist: ["es-cn-09k1rgid9000g****.elasticsearch.aliyuncs.com:9200"]
            ```

        -   对于多可用区实例，白名单需要配置为实例中所有数据节点的IP地址与端口的组合。

            ![多可用区远程白名单配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7202659951/p135853.png)

            ```
            reindex.remote.whitelist: ["10.0.xx.xx:9200","10.0.xx.xx:9200","10.0.xx.xx:9200","10.15.xx.xx:9200","10.15.xx.xx:9200","10.15.xx.xx:9200"]
            ```

            **说明：** 您可以在实例**基本信息**页面的**节点可视化**页签中，获取实例中所有数据节点的IP地址。详细信息，请参见[查看节点的基本信息](/intl.zh-CN/Elasticsearch/管理实例/查看集群状态和节点信息.md)。

    4.  勾选**该操作会重启实例，请确认后操作**，单击**确定**。

5.  实例重启完成后，配置本地和远程实例网络互通。

    在本地集群中，添加需要进行网络互通的远程集群。具体操作，请参见[配置实例网络互通](/intl.zh-CN/Elasticsearch/安全配置/配置实例网络互通.md)。

    ![配置实例网络互通](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7202659951/p135740.png)

6.  在本地集群中，调用reindex API重建索引。

    参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)，执行如下命令，重建索引。

    ```
    POST _reindex
    {
      "source": {
        "remote": {
          "host": "http://es-cn-09k1rgid9000g****.elasticsearch.aliyuncs.com:9200",
          "username": "elastic",
          "password": "your_password"
        },
        "index": "product_info",
        "query": {
          "match": {
            "productName": "理财"
          }
        }
      },
      "dest": {
        "index": "dest"
      }
    }
    ```

    |类别|参数|说明|
    |--|--|--|
    |source|host|远程集群的访问地址，必须包含支持协议、域名和端口信息，例如https://otherhost:9200。    -   对于单可用区实例，host格式为http://<实例的域名\>:9200。

**说明：** 实例的域名可在基本信息页面获取。详细信息，请参见[查看实例的基本信息](/intl.zh-CN/Elasticsearch/管理实例/查看实例的基本信息.md)。

    -   对于多可用区实例，host格式为http://<实例中任意数据节点的IP地址\>:9200。 |
    |username|可选参数，如果您所请求的远程Elasticsearch服务需要使用Basic Authentication，请在请求中一并提供此参数信息。阿里云Elasticsearch实例的默认用户名为elastic。**说明：**

    -   为确保安全性，通过Basic Authentication鉴权时建议使用HTTPS协议，否则密码信息将以文本形式进行传输。
    -   对于阿里云Elasticsearch实例，需要开启HTTPS协议后，才可在host中使用HTTPS协议。 |
    |password|用户对应的密码。阿里云Elasticsearch实例的elastic用户的密码在创建实例时设定，如果忘记可进行重置。重置密码的注意事项及具体操作，请参见[重置实例访问密码](/intl.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|
    |index|远程集群中的源索引。|
    |query|通过查询语法，指定待迁移的数据。详细信息，请参见[Reindex API](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/docs-reindex.html?spm=a2c4g.11186623.2.16.4cd96ac0AD7xUw)。|
    |dest|index|本地集群中的目标索引。|

    **说明：** 从远程集群重建索引数据，不支持手动切片或自动切片。详细信息，请参见[手动切片](https://github.com/elastic/elasticsearch/blob/5.6/docs/reference/docs/reindex.asciidoc?spm=a2c4g.11186623.2.17.4cd96ac0AD7xUw#docs-reindex-manual-slice)和[自动切片](https://github.com/elastic/elasticsearch/blob/5.6/docs/reference/docs/reindex.asciidoc?spm=a2c4g.11186623.2.18.4cd96ac0AD7xUw#docs-reindex-automatic-slice)。

    执行成功后，返回如下结果。

    ```
    {
      "took" : 51,
      "timed_out" : false,
      "total" : 2,
      "updated" : 2,
      "created" : 0,
      "deleted" : 0,
      "batches" : 1,
      "version_conflicts" : 0,
      "noops" : 0,
      "retries" : {
        "bulk" : 0,
        "search" : 0
      },
      "throttled_millis" : 0,
      "requests_per_second" : -1.0,
      "throttled_until_millis" : 0,
      "failures" : [ ]
    }
    ```

7.  查看迁移成功的数据。

    ```
    GET dest/_search
    ```

    返回结果如下：

    -   单可用区实例

        ![查看迁移成功的数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5702852061/p135751.png)

    -   多可用区实例

        ![查看迁移成功的数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6702852061/p135865.png)


## 总结

通过reindex API迁移数据时，单可用区的阿里云Elasticsearch实例和多可用区实例的配置方法大致相同，不同之处在于以下两点。

|可用区类型|reindex白名单配置|host参数配置|
|-----|------------|--------|
|单可用区|阿里云Elasticsearch的域名:9200|https://阿里云Elasticsearch实例的域名:9200|
|多可用区|实例中所有数据节点的IP地址与端口的组合|https://阿里云Elasticsearch实例中任意数据节点的IP地址:9200|

## 更多信息

在调用reindex API重建索引时，您还可以进行批量设置和超时时间设置：

-   批量设置

    远程Elasticsearch集群使用堆缓存索引数据，默认最大设定值为100MB。如果远程索引中包含大文档，请将批量数值设置为较小值。

    以下示例中，通过size设置批量数值为10。

    ```
    POST _reindex
    {
      "source": {
        "remote": {
          "host": "http://otherhost:9200"
        },
        "index": "source",
        "size": 10,
        "query": {
          "match": {
            "test": "data"
          }
        }
      },
      "dest": {
        "index": "dest"
      }
    }
    ```

-   超时时间设置

    您可以使用socket\_timeout设置socket读取超时时间，默认为30s；使用connect\_timeout设置连接超时时间，默认为1s。

    以下示例中，设置socket读取超时时间为1分钟，连接超时时间为10秒。

    ```
    POST _reindex
    {
      "source": {
        "remote": {
          "host": "http://otherhost:9200",
          "socket_timeout": "1m",
          "connect_timeout": "10s"
        },
        "index": "source",
        "query": {
          "match": {
            "test": "data"
          }
        }
      },
      "dest": {
        "index": "dest"
      }
    }
    ```


