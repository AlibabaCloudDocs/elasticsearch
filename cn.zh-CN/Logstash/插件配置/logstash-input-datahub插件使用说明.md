---
keyword: [logstash-input-datahub插件, logstash读取datahub数据]
---

# logstash-input-datahub插件使用说明

通过logstash-input-datahub插件，您可以读取DataHub中的数据到其他数据源中。本文介绍如何使用logstash-input-datahub插件。

## 前提条件

您已完成以下操作：

-   安装logstash-input-datahub插件。

    详情请参见[安装Logstash插件](/cn.zh-CN/Logstash/插件配置/安装Logstash插件.md)。

-   开通DataHub产品，并完成创建项目、创建Topic和导入数据。

    详细信息，请参见DataHub官方文档的[用户指南](https://help.aliyun.com/document_detail/158790.html?spm=a2c4g.11186623.6.558.2001a8cbPy7kRt)章节。


## 使用logstash-input-datahub插件

满足以上[前提条件](#section_1zh_hzl_7sh)后，您可以[通过配置文件管理管道](/cn.zh-CN/Logstash/管道任务管理/通过配置文件管理管道.md)的方式创建管道任务。在创建管道任务时，按照以下说明配置Pipeline参数，保存并部署后，即可触发阿里云Logstash读取DataHub的数据到目标数据源中。

配置脚本如下，相关参数说明请参见[参数说明](#section_ihp_ccv_ux0)。

```
input {
    datahub {
        access_id => "Your accessId"
        access_key => "Your accessKey"
        endpoint => "Endpoint"
        project_name => "test_project"
        topic_name => "test_topic"
        interval => 5
        #cursor => {
        #    "0"=>"20000000000000000000000003110091"
        #    "2"=>"20000000000000000000000003110091"
        #    "1"=>"20000000000000000000000003110091"
        #    "4"=>"20000000000000000000000003110091"
        #    "3"=>"20000000000000000000000003110000"
        #}
        shard_ids => []
        pos_file => "/ssd/1/<Logstash实例ID>/logstash/data/文件名"
    }
}
output {
    elasticsearch {
      hosts => ["http://es-cn-mp91cbxsm000c****.elasticsearch.aliyuncs.com:9200"]
      user => "elastic"
      password => "your_password"
      index => "datahubtest"
      document_type => "_doc"
  }
}
```

**说明：** 目前阿里云Logstash只支持同一专有网络VPC（Virtual Private Cloud）下的数据传输，如果源端数据在公网环境下，请参见[配置NAT公网数据传输](/cn.zh-CN/Logstash/网络与安全/配置NAT公网数据传输.md)，通过公网访问Logstash。

## 参数说明

**logstash-input-datahub**插件支持的参数如下。

|参数|类型|是否必选|说明|
|--|--|----|--|
|`endpoint`|string|是|DataHub对外服务的访问域名，详细信息请参见[DataHub域名列表](https://help.aliyun.com/document_detail/158778.html)。|
|`access_id`|string|是|阿里云账号的AccessKey ID。|
|`access_key`|string|是|阿里云账号的Access Key Secret。|
|`project_name`|string|是|DataHub的项目名称。|
|`topic_name`|string|是|DataHub的Topic名称。|
|`retry_times`|number|否|重试次数。-1表示无限重试（默认）、0表示不重试、大于0表示按照设置的次数重试。|
|`retry_interval`|number|否|重试的间隔，单位为秒。|
|`shard_ids`|array|否|需要消费的shard列表。默认为空，表示全部消费。|
|`cursor`|string|否|消费起点。默认为空，表示从头开始消费。|
|`pos_file`|string|是|Checkpoint记录文件，必须配置，优先使用checkpoint恢复消费。|
|`enable_pb`|boolean|否|是否使用pb传输，默认为true。如果不支持pb传输，请将该参数设置为false。|
|`compress_method`|string|否|网络传输的压缩算法，默认不压缩。可选项：`lz4`、`deflate`。|
|`print_debug_info`|boolean|否|是否打印DataHub的Debug信息，默认为false。设置为true时，会打印大量信息，这些信息仅用来进行脚本调试。|

