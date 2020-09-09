---
keyword: [logstash-output-datahub插件, logstash批量传送数据到datahub]
---

# logstash-output-datahub插件使用说明

通过logstash-output-datahub插件，您可以将数据传输到DataHub中。

## 前提条件

您已完成以下操作：

-   安装logstash-output-datahub插件。

    详情请参见[安装Logstash插件](/cn.zh-CN/Logstash实例/插件配置/安装Logstash插件.md)。

-   开通DataHub产品，并完成创建项目和创建Topic。

    详情请参见DataHub官方文档的[用户指南](https://help.aliyun.com/document_detail/158789.html)章节。

-   准备输入数据源。

    输入数据源可以为input支持的所有输入源插件中的数据，本文以阿里云Elasticsearch为例，详情请参见[input插件](https://www.elastic.co/guide/en/logstash/6.7/input-plugins.html)。


## 使用logstash-output-datahub插件

满足以上[前提条件](#section_a43_2zx_hks)后，您可以[通过配置文件管理管道](/cn.zh-CN/Logstash实例/管道任务管理/通过配置文件管理管道.md)的方式创建管道任务。在创建管道任务时，按照以下说明配置Pipeline参数，保存并部署后，即可触发阿里云Logstash向DataHub传送数据。

Logstash的Pipeline配置如下，相关参数说明请参见[参数说明](#section_4un_f2g_0rp)。

```
input {
    elasticsearch {
      hosts => ["http://es-cn-mp91cbxsm000c****.elasticsearch.aliyuncs.com:9200"]
      user => "elastic"
      index => "test"
      password => "your_password"
      docinfo => true
  }
}
filter{
    
}
output {
    datahub {
        access_id => "Your accessId"
        access_key => "Your accessKey"
        endpoint => "Endpoint"
        project_name => "project"
        topic_name => "topic"
        #shard_id => "0"
        #shard_keys => ["thread_id"]
        dirty_data_continue => true
        dirty_data_file => "/Users/ph0ly/trash/dirty.data"
        dirty_data_file_max_size => 1000
    }
}
```

**说明：** 阿里云Logstash目前只支持在同一专有网络VPC（Virtual Private Cloud）下进行数据传输，如果源端数据在公网下，请参见[配置NAT公网数据传输](/cn.zh-CN/Logstash实例/网络与安全/配置NAT公网数据传输.md)，在公网环境下进行数据传输。

## 参数说明

**logstash-output-datahub**插件支持的参数如下。

|参数|类型|是否必选|说明|
|--|--|----|--|
|`endpoint`|string|是|DataHub对外服务的访问域名，详情请参见[DataHub域名列表](https://help.aliyun.com/document_detail/158778.html)。|
|`access_id`|string|是|阿里云账号的AccessKey ID。|
|`access_key`|string|是|阿里云账号的Access Key Secret。|
|`project_name`|string|是|DataHub的项目名称。|
|`topic_name`|string|是|DataHub的Topic名称。|
|`retry_times`|number|否|重试次数。-1表示无限重试（默认）、0表示不重试、大于0表示按照设置的次数重试。|
|`retry_interval`|number|否|重试的间隔，单位为秒，默认为5。|
|`skip_after_retry`|boolean|否|当由DataHub异常导致的重试次数超过`retry_times`设置的值，是否跳过这一轮上传的数据。默认为false。|
|`approximate_request_bytes`|number|否|用于限制每次发送请求的字节数，是一个近似值，防止因Request body过大而被拒绝接收，默认为2048576（2MB）。|
|`shard_keys`|array|否|数据的字段名称，插件会根据这些字段的值计算Hash值，将每条数据写入到某个shard。 **说明：** `shard_keys`和`shard_ids`都未指定，默认轮询写入各shard。 |
|`shard_ids`|array|否|所有数据写入指定的shard。 **说明：** `shard_keys`和`shard_ids`都未指定，默认轮询写入各shard。 |
|`dirty_data_continue`|string|否|处理数据时遇到脏数据是否继续运行，默认为false。设置为true时，必须指定`dirty_data_file`文件，表示处理数据时忽略脏数据。|
|`dirty_data_file`|string|否|脏数据文件名称。当`dirty_data_continue`为true时，必须指定该参数值。 **说明：** 处理数据时，脏数据文件会被分割成两个部分part1和part2，part1为原脏数据，part2为替换后的脏数据。 |
|`dirty_data_file_max_size`|number|否|脏数据文件大小的最大值。|
|`enable_pb`|boolean|否|是否使用pb传输，默认为true。如果不支持pb传输，请将该参数值设置为false。|

