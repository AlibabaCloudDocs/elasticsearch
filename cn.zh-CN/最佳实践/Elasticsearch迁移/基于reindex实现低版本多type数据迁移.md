# 基于reindex实现低版本多type数据迁移

本文介绍基于reindex将阿里云Elasticsearch 5.x实例中的多type数据，迁移到高版本Elasticsearch 6.x实例的单type索引中。

## 注意事项

由于阿里云Elasticsearch网络架构调整，对创建的实例有以下影响：

-   2020年10月及之后创建的实例，暂不支持Watcher报警和LDAP认证功能。
-   2020年10月及之后创建的实例，不支持与10月之前创建的实例进行跨集群Reindex、跨集群搜索、跨集群复制等相关操作。如果需要使用跨集群操作，需要确保实例创建在同一网络架构下。

**说明：** 阿里云Elasticsearch在华北3（张家口）、海外地域的网络架构调整时间在2020年10月之前，如果需要使用跨集群操作，请[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex)联系技术支持同学校验网络架构是否可以使用。

## 操作流程

1.  [准备工作](#section_vdv_hqt_jjm)

    准备阿里云Elasticsearch和Logstash实例，确保两者在同一专有网络下。

    -   阿里云Elasticsearch实例：存储索引数据。
    -   阿里云Logstash实例：通过管道配置功能，迁移处理后的数据。
2.  [步骤一：转换索引类型](#section_y7t_p2z_5is)

    通过reindex，将阿里云Elasticsearch 5.x实例中的多type索引转换为单type索引。您可以通过以下两种方式来实现：

    -   合并type方式：将Elasticsearch 5.x实例中的单索引多type数据，通过reindex script方式合并成一个单索引单type数据。
    -   拆分type方式：将Elasticsearch 5.x实例中的单索引多type数据，按照不同的type，通过reindex拆分成多个单索引单type数据的方式。
3.  [步骤二：通过Logstash迁移数据](#section_qih_65i_yb0)

    使用阿里云Logstash，将处理后的索引数据迁移至高版本Elasticsearch 6.x实例中。

4.  [步骤三：查看数据迁移结果](#section_eyh_unl_ywa)

    在Kibana中查看迁移成功的索引。


## 准备工作

1.  准备低版本（5.5.3）和高版本（6.7.0）的阿里云Elasticsearch实例，并准备待迁移的多type数据。

    创建实例的具体操作，请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

2.  创建阿里云Logstash实例，要求与阿里云Elasticsearch实例处于同一专有网络下。

    具体操作，请参见[创建阿里云Logstash实例](/cn.zh-CN/Logstash/快速入门/步骤一：创建实例/创建阿里云Logstash实例.md)。


## 步骤一：转换索引类型

以下步骤介绍通过合并type方式，将单索引多type数据合并成一个单索引单type数据。

1.  开启Elasticsearch实例的自动创建索引功能。

    1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

    2.  在左侧导航栏，单击**Elasticsearch实例**。

    3.  在顶部菜单栏处，选择资源组和地域。

    4.  在**实例列表**中，单击低版本的实例ID。

    5.  在左侧导航栏，单击**ES集群配置**。

    6.  单击**YML文件配置**右侧的**修改配置**。

    7.  在**YML文件配置**页面，设置**自动创建索引**为**允许自动创建索引**。

        ![允许自动创建索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9302659951/p74665.png)

        **警告：** 修改**自动创建索引**方式会触发实例重启，请确认后再操作。

    8.  勾选**该操作会重启实例，请确认后操作**，单击**确定**。

2.  登录低版本Elasticsearch实例的Kibana控制台。

    具体操作，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

3.  在左侧导航栏，单击**Dev Tools**（开发工具）。

4.  在**Console**中，执行以下命令，将单索引多type数据合并成单索引单type数据。

    ```
    POST _reindex
    {
      "source": {
        "index": "twitter"
      },
      "dest": {
        "index": "new1"
      },
      "script": {
        "inline": """
        ctx._id = ctx._type + "-" + ctx._id;
        ctx._source.type = ctx._type;
        ctx._type = "doc";
        """,
        "lang": "painless"
      }
    }
    ```

    以上示例通过自定义type的方式，指定ctx.\_source.type在new1索引中添加type字段，将其设置为原始\_type的值。并且new1索引的\_id由\_type-\_id组成，防止存在不同类型的文档具有相同的ID而发生冲突的情况。

5.  执行`GET new1/_mapping`命令，查看合并后的Mapping结构。

6.  执行以下命令，查看合并后的索引数据。

    ```
    GET new1/_search
    {
       "query":{
         "match_all":{
          }
      }
    }
    ```


以下步骤介绍通过拆分type方式，将单索引多type数据，按照不同的type，通过reindex拆分成多个单索引单type数据。

1.  在**Console**中，执行以下命令，将单索引多type拆分单索引单type。

    ```
    POST _reindex
    {
      "source": {
        "index": "twitter",
        "type": "tweet",
        "size": 10000
      },
      "dest": {
        "index": "twitter_tweet"
      }
    }
    POST _reindex
    {
      "source": {
        "index": "twitter",
        "type": "user",
        "size": 10000
      },
      "dest": {
        "index": "twitter_user"
      }
    }
    ```

    以上示例将twitter索引按照不同type，分别拆分成twitter\_tweet和twitter\_user索引。

2.  执行以下命令，查看拆分后的索引数据。

    ```
    GET twitter_tweet/_search
    {
       "query":{
         "match_all":{
    
          }
      }
    }
    ```

    ```
    GET twitter_user/_search
    {
       "query":{
         "match_all":{
    
          }
      }
    }
    ```


## 步骤二：通过Logstash迁移数据

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Logstash实例**。

3.  在顶部菜单栏处，选择地域，然后在**Logstash实例**中单击目标实例ID。

4.  在左侧导航栏，单击**管道管理**。

5.  在**管道管理**页面，使用**配置文件管理**方式配置管道。

    管道配置的具体操作，请参见[通过配置文件管理管道](/cn.zh-CN/Logstash/管道任务管理/通过配置文件管理管道.md)，Config配置示例如下。

    ```
    input {
        elasticsearch {
        hosts => ["http://es-cn-0pp1f1y5g000h****.elasticsearch.aliyuncs.com:9200"]
        user => "elastic"
        index => "*"
        password => "your_password"
        docinfo => true
      }
    }
    filter {
    }
    output {
      elasticsearch {
        hosts => ["http://es-cn-mp91cbxsm000c****.elasticsearch.aliyuncs.com:9200"]
        user => "elastic"
        password => "your_password"
        index => "test"
      }
    }
    ```

6.  保存并部署管道配置，开始迁移数据。

    具体操作，请参见[通过配置文件管理管道](/cn.zh-CN/Logstash/管道任务管理/通过配置文件管理管道.md)。


## 步骤三：查看数据迁移结果

1.  登录高版本Elasticsearch实例的Kibana控制台。

    具体操作，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**。

3.  在**Console**中，执行以下命令，查看迁移成功的索引。

    ```
    GET _cat/indices?v
    ```


