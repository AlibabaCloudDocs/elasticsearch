# 通过API操作阿里云Elasticsearch

开源Elasticsearch提供了一系列Restful风格的API，您可以通过curl命令使用，也可以在Kibana中使用。本文介绍如何通过curl命令，调用API与阿里云Elasticsearch实例进行交互，并完成查看集群信息、创建索引和文档、搜索文档等操作。

创建一个与阿里云Elasticsearch在同一专有网络VPC（Virtual Private Cloud）下的云服务器ECS实例。具体操作，请参见[使用向导创建实例](/intl.zh-CN/实例/创建实例/使用向导创建实例.md)。

**说明：** 您也可以使用已创建的ECS实例，但需确保与Elasticsearch实例在相同VPC下。如果ECS处于经典网络下，期望访问VPC内的Elasticsearch，需要先参考[通过经典网络访问ES常见问题](/intl.zh-CN/Elasticsearch/常见问题/通过经典网络访问ES常见问题.md)进行相关配置。

本文介绍通过curl命令使用API的方法，Kibana方式请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

## 访问实例

1.  连接ECS实例。

    具体操作，请参见[步骤三：连接ECS实例](/intl.zh-CN/快速入门/通过控制台创建实例（详细版）/Linux系统实例快速入门.md)。

2.  使用如下命令，访问阿里云Elasticsearch实例。

    **说明：** 如果系统提示`curl command not found`，请先使用`yum install curl`命令，在ECS中安装curl。

    ```
    curl -u <user>:<password> http://<host>:<port>
    ```

    |变量名|说明|
    |---|--|
    |<user\>|阿里云Elasticsearch实例的访问用户名。建议通过非elastic账号访问。**说明：**

    -   支持通过elastic账号访问，但因为在修改elastic账号的密码后需要一些时间来生效，在密码生效期间会影响服务访问，因此不建议通过elastic账号来访问。建议在Kibana控制台中创建一个符合预期的Role角色用户进行访问，具体操作请参见[创建角色](/intl.zh-CN/访问控制/Kibana角色管理/创建角色.md)和[创建用户](/intl.zh-CN/访问控制/Kibana角色管理/创建用户.md)。
    -   如果您创建的阿里云Elasticsearch实例的版本中包含with\_X-Pack信息，则访问该实例时，必须指定用户名和密码。 |
    |<password\>|对应账号的密码。elastic账号的密码在创建实例时设定，如果忘记可重置。重置密码的注意事项和具体操作，请参见[重置实例访问密码](/intl.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|
    |<host\>|实例的**内网地址**。可在实例的[基本信息](/intl.zh-CN/Elasticsearch/管理实例/查看实例的基本信息.md)页面获取。|
    |<port\>|实例的访问端口。一般为**9200**，可在实例的[基本信息](/intl.zh-CN/Elasticsearch/管理实例/查看实例的基本信息.md)页面获取。|

    访问示例如下。

    ```
    curl -u <user>:<password> http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200
    ```

    访问成功后，返回结果如下。

    ![使用ECS访问ES结果示例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1869559951/p58858.png)


## 查看集群信息

-   查看集群健康状况

    ```
    curl -u <user>:<password> -XGET 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/_cat/health?v'
    ```

    执行成功后，返回如下结果。

    ![查看集群健康状况](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1869559951/p88445.png)

-   查看集群中包含的索引信息

    ```
    curl -u <user>:<password> -XGET 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/_cat/indices?v'
    ```

    执行成功后，返回如下结果。

    ![查看集群索引信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1869559951/p88448.png)


## 创建索引和文档

-   创建索引

    ```
    curl -u <user>:<password> -XPUT 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info'
    ```

    以上示例创建了一个名称为`product_info`的索引。

    执行成功后，返回如下结果。

    ![创建索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1869559951/p88449.png)

-   为索引设置mapping

    ```
    curl -u <user>:<password> -XPUT 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info/_doc/_mapping' -H 'Content-Type: application/json' -d '
    {
     "_doc":{
       "properties": {
            "productName": {"type": "text","analyzer": "ik_smart"},
            "annual_rate":{"type":"keyword"},
            "describe": {"type": "text","analyzer": "ik_smart"}
          }
      }
    }'
    ```

    执行成功后，返回如下结果。

    ![为索引设置mapping](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1869559951/p88464.png)

    以上示例设置`product_info`索引的类型为`_doc`，包含了`productName`、`annual_rate`和`describe`字段，并定义了各字段的类型所使用的分词器。

-   创建文档并插入数据
    -   创建单个文档

        ```
        curl -u <user>:<password> -XPOST 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info/_doc/1?pretty' -H 'Content-Type: application/json' -d '
        {
        "productName":"testpro",
        "annual_rate":"3.22%",
        "describe":"testpro"
        }'
        ```

        执行成功后，返回结果如下。

        ![创建文档并插入数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1869559951/p88456.png)

        以上示例在类型为`_doc`的`product_info`索引中，创建了一个名称为`1`的文档，并向文档中插入了一条数据。

    -   创建多个文档

        ```
        curl -u <user>:<password> -XPOST http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/_bulk -H 'Content-Type: application/json' -d'
        { "index" : { "_index": "product_info", "_type" : "_doc", "_id" : "1" } }
        {"productName":"testpro","annual_rate":"3.22%","describe":"testpro"}
        { "index" : { "_index": "product_info", "_type" : "_doc", "_id" : "2" } }
        {"productName":"testpro1","annual_rate":"3.26%","describe":"testpro"}'
        ```

        执行成功后，返回结果如下。

        ![创建多个文档](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7904440161/p212941.png)

        以上示例在类型为`_doc`的`product_info`索引中，创建了一个名称为`1`和`2`的文档，并分别向文档中插入了一条数据。


## 搜索文档

```
curl -u <user>:<password> -XGET 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info/_doc/1?pretty'
```

执行成功后，返回结果如下。

![搜索文档](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1869559951/p88461.png)

以上示例搜索名称为`1`的文档。

## 删除索引

```
curl -u <user>:<password> -XDELETE 'http://es-cn-vxxxxx****.elasticsearch.aliyuncs.com:9200/product_info'
```

执行成功后，返回结果如下。

![删除索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1869559951/p88462.png)

以上示例删除了名称为`product_info`的索引。

**说明：** 更多命令，请参见[Elasticsearch官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/current/rest-apis.html)。

