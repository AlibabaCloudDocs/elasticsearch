---
keyword: Flink和es构建日志检索系统
---

# 通过实时计算处理数据并同步到Elasticsearch

当您需要构建一个日志检索系统时，可通过实时计算Flink对日志数据进行计算后，输出到Elasticsearch进行搜索。本文以阿里云日志服务SLS（Log Service）为例，为您介绍具体的实现方法。

您已完成以下操作：

-   开通阿里云实时计算服务并创建项目。

    具体操作步骤请参见[开通服务和创建项目](/intl.zh-CN/独享模式/准备工作/开通服务和创建项目.md)。

-   创建阿里云Elasticsearch实例。

    具体操作步骤请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

-   开通SLS服务、创建Project和Logstore。

    具体操作步骤请参见[开通阿里云日志服务](/intl.zh-CN/快速入门/快速入门.md)、[创建Project](/intl.zh-CN/数据采集/准备工作/管理Project.md)[创建Logstore](t13024.md#section_v52_2jx_ndb)。


阿里云实时计算Flink是阿里云官方支持的Flink产品，支持包括Kafka、Elasticsearch等多种输入输出系统。实时计算Flink与Elasticsearch结合，能够满足典型的日志检索场景。

Kafka或LOG等系统中的日志，经过Flink进行简单或者复杂计算之后，输出到Elasticsearch进行搜索。结合Flink的强大计算能力与Elasticsearch的强大搜索能力，可为业务提供实时数据加工及查询，助力业务实时化转型。

实时计算Flink为您提供了非常简单的方式来对接Elasticsearch。例如当前业务中的日志或者数据被写入了LOG中，并且需要对LOG中的数据进行计算之后再写到Elasticsearch中进行搜索，可通过以下链路实现。

![Flink+ES数据链路](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/7724659951/p62597.png)

## 操作步骤

1.  登录[实时计算控制台](https://stream-ap-southeast-3.console.aliyun.com)。

2.  创建实时计算作业。

    具体操作步骤请参见[创建作业](/intl.zh-CN/独享模式/Flink SQL开发指南/作业开发/开发.md)。

3.  编写Flink SQL。

    1.  创建日志服务LOG源表。

        ```
        create table sls_stream(
          a int,
          b int,
          c VARCHAR
        )
        WITH (
          type ='sls',  
          endPoint ='<yourEndpoint>',
          accessId ='<yourAccessId>',
          accessKey ='<yourAccessKey>',
          startTime = '<yourStartTime>',
          project ='<yourProjectName>',
          logStore ='<yourLogStoreName>',
          consumerGroup ='<yourConsumerGroupName>'
        );
        ```

        WITH参数说明如下表。

        |变量|说明|
        |--|--|
        |endPoint|阿里云日志服务的公网服务入口，即访问对应LOG项目及其内部日志数据的URL。详情请参见[服务入口](/intl.zh-CN/开发指南/API 参考/服务入口.md)。 例如杭州区域的日志服务入口为：**http://cn-hangzhou.log.aliyuncs.com**。需要在对应的服务入口前加**http://**。 |
        |accessId|您账号的AccessKey ID。|
        |accessKey|您账号的AccessKey Secret。|
        |startTime|消费日志开始的时间点。运行Flink作业时所选时间要大于此处设置的时间。|
        |project|LogService的项目名称。|
        |logStore|LogService项目下具体的LogStore名称。|
        |consumerGroup|日志服务的消费组名称。|

        更多WITH参数及其说明请参见[创建日志服务SLS源表](/intl.zh-CN/独享模式/Flink SQL参考/DDL语句/创建数据源表/创建日志服务LOG源表.md)。

    2.  创建Elasticsearch结果表。

        **说明：**

        -   实时计算3.2.2及以上版本增加了Elasticsearch结果表功能。创建Flink作业时，请注意所选的版本。
        -   Elasticsearch结果表的实现使用了REST API，可以兼容Elasticsearch的各个版本。
        ```
        CREATE TABLE es_stream_sink(
          a int,
          cnt BIGINT,
          PRIMARY KEY(a)
        )
        WITH(
          type ='elasticsearch',
          endPoint = 'http://<instanceid>.public.elasticsearch.aliyuncs.com:<port>',
          accessId = '<yourAccessId>',
          accessKey = '<yourAccessSecret>',
          index = '<yourIndex>',
          typeName = '<yourTypeName>'
        );
        ```

        WITH参数说明如下。

        |参数|说明|
        |--|--|
        |endPoint|阿里云Elasticsearch实例的公网地址，格式为http://<instanceid\>.public.elasticsearch.aliyuncs.com:9200。可在实例的基本信息页面获取，详情请参见[查看实例的基本信息](/intl.zh-CN/实例管理/管理实例/查看实例的基本信息.md)。|
        |accessId|访问阿里云Elasticsearch实例的用户名，默认为elastic。|
        |accessKey|对应用户的密码。elastic用户的密码在创建实例时设定，如果忘记可进行重置，重置密码的注意事项和操作步骤请参见[重置实例访问密码](/intl.zh-CN/实例管理/安全配置/重置实例访问密码.md)。|
        |index|索引名称。如果您还未创建过索引，需要先创建一个索引，详情请参见[创建索引](/intl.zh-CN/快速入门/业务查询/创建索引.md)。您也可以开启自动创建索引功能，自动创建对应索引，详情请参见[开启自动创建索引](/intl.zh-CN/快速入门/步骤二：配置实例（可选）.md)。|
        |typeName|索引类型。7.0及以上版本的Elasticsearch实例必须为\_doc。|

        更多WITH参数及其说明请参见[创建Elasticsearch结果表](/intl.zh-CN/独享模式/Flink SQL参考/DDL语句/创建数据结果表/创建Elasticsearch结果表.md)。

        **说明：**

        -   Elasticsearch支持根据`PRIMARY KEY`更新文档，且`PRIMARY KEY`只能为1个字段。指定`PRIMARY KEY`后，文档的ID为`PRIMARY KEY`字段的值。未指定`PRIMARY KEY`，文档的ID由系统随机生成，详情请参见[Index API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html)。
        -   Elasticsearch支持多种更新模式，对应WITH中的参数为`updateMode`：
            -   当`updateMode=full`时，新增的文档会完全覆盖已存在的文档。
            -   当`updateMode=inc`时，Elasticsearch会根据输入的字段值更新对应的字段。
        -   Elasticsearch所有的更新默认为UPSERT语义，即INSERT或UPDATE。
    3.  处理业务逻辑并同步数据。

        ```
        INSERT INTO es_stream_sink
        SELECT 
          a,
          count(*) as cnt
        FROM sls_stream GROUP BY a
        ```

4.  上线并启动作业。

    具体操作步骤请参见[作业上线]()和[生产运维]()。

    上线并启动作业后，即可将日志服务中的数据进行简单聚合后写入阿里云Elasticsearch中。实时计算Flink还支持更多的计算操作，详情请参见[概述](/intl.zh-CN/独享模式/Flink SQL参考/Flink SQL概述.md)。


## 更多信息

使用实时计算Flink+Elasticsearch，可帮助您快速创建实时搜索链路。如果您有更复杂的Elasticsearch写入需求，可以使用实时计算Flink的自定义Sink功能来实现，详情请参见[创建自定义结果表](/intl.zh-CN/独享模式/Flink SQL参考/DDL语句/创建数据结果表/创建自定义结果表.md)。

