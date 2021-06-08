---
keyword: [logstash-input-oss, Logstash OSS事件通知, Logstash MNS]
---

# logstash-input-oss插件使用说明

logstash-input-oss插件基于阿里云消息服务MNS（Message Notification Service），实现了当关联的对象存储服务OSS（Object Storage Service）文件变化时，触发MNS通知阿里云LogstashService（简称Logstash）从OSS文件系统中获取最新的数据。您可以在OSS的事件通知区域，配置当文件发生变化时，自动发送消息给MNS。

**说明：** logstash-input-oss是阿里云维护的开源插件，详情请参见[logstash-input-oss](https://github.com/aliyun/logstash-input-oss)。

## 注意事项

-   当**logstash-input-oss**插件接收到MNS通知消息后，阿里云Logstash会全量同步关联的文件。
-   如果OSS存储的是.gz或.gzip结尾的文本文件，阿里云Logstash会以.gzip的文件格式对其进行处理，其他格式的文件以文本文件进行处理。
-   文件是以文本文件的方式读取的，如果您的文件是不可解析的格式（例如.jar、.bin等格式），有可能读取出来是乱码。

## 前提条件

您已完成以下操作：

-   安装logstash-input-oss插件。

    详情请参见[安装Logstash插件](/intl.zh-CN/Logstash/插件配置/安装Logstash插件.md)。

-   开通阿里云OSS服务和阿里云MNS服务，且两者在相同区域。

    详情请参见[开通阿里云OSS服务](/intl.zh-CN/控制台用户指南/开通OSS服务.md)和[MNS服务](https://www.alibabacloud.com/help/zh/doc-detail/27423.htm)。

-   在OSS中配置事件通知。

    详情请参见[设置事件通知规则](/intl.zh-CN/控制台用户指南/存储空间管理/基础设置/设置事件通知规则.md)。


## 使用logstash-input-oss插件

满足以上前提条件后，您可以[通过配置文件管理管道](/intl.zh-CN/Logstash/管道任务管理/通过配置文件管理管道.md)的方式创建管道任务。在创建管道任务时，请按照以下说明配置管道参数。配置完成后进行保存与部署，即可触发阿里云Logstash从OSS中获取数据。

以从OSS中获取数据，然后写入到阿里云Elasticsearch（简称ES）为例，配置示例如下。

```
input {
  oss {
    endpoint => "oss-cn-hangzhou-internal.aliyuncs.com"
    bucket => "zl-ossou****"
    access_key_id => "******"
    access_key_secret => "*********" 
    prefix => "file-sample-prefix"
    mns_settings => {
       endpoint => "******.mns.cn-hangzhou-internal.aliyuncs.com"
       queue => "aliyun-es-sample-mns"
    }
    codec => json {
      charset => "UTF-8"
    }
  }
}

output {
  elasticsearch {
    hosts => ["http://es-cn-***.elasticsearch.aliyuncs.com:9200"]
    index => "aliyun-es-sample"
    user => "elastic"
    password => "changeme"
  }
}
```

**说明：** MNS Endpoint不能以HTTP为前缀，并且需要internal域名，否则会报错。

## 参数说明

**logstash-input-oss**插件支持的参数如下。

|参数|类型|是否必选|说明|
|--|--|----|--|
|`endpoint`|string|是|OSS对外服务的访问域名，详情请参见[访问域名和数据中心](/intl.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。|
|`bucket`|string|是|OSS的Bucket名称。|
|`access_key_id`|string|是|阿里云账号的AccessKey ID。|
|`access_key_secret`|string|是|阿里云账号的Access Key Secret。|
|`prefix`|string|否|如果指定了该参数，则Bucket中文件名的前缀必须与之匹配（不是正则表达式）。|
|`additional_oss_settings`|hash|否|附加的OSS客户端配置。可选值：`secure_connection_enabled`和`max_connections_to_oss`。|
|`delete`|boolean|否|是否从原始Bucket中删除已处理的文件。默认为`false`。|
|`backup_to_bucket`|string|否|用来备份已处理过的文件的OSS Bucket名称。|
|`backup_to_dir`|string|否|用来备份已经处理过的文件的本地目录路径。|
|`backup_add_prefix`|string|否|文件处理后，为key（OSS中包含文件名的完整路径）附加一个前缀。当您将数据备份到另一个（或同一个）Bucket时，这个参数将有效地让您选择一个新的文件夹来放置文件。|
|`include_object_properties`|boolean|否|是否在`[@metadata][oss]`中包含OSS对象的属性（last\_modified，content\_type，metadata）。如果不设置此参数，`[@metadata][oss][key]`将始终存在。|
|`exclude_pattern`|string|否|要从Bucket中排除的key的ruby正则表达式。|
|`mns_settings`|hash|是|消息服务（MNS）配置。 可选值及说明如下：

-   `endpoint`：MNS端口链接。不能以HTTP为前缀，并且需要internal域名，否则会报错。
-   `queue`：队列名。
-   `poll_interval_seconds`：当队列中没有消息时，针对该队列的ReceiveMessage请求最长的等待时间，默认为10秒。
-   `wait_seconds`：本次ReceiveMessage请求最长的Polling等待时间，单位为秒。

ReceiveMessage的详细信息请参见[ReceiveMessage](https://www.alibabacloud.com/help/zh/doc-detail/35136.html)。 |

## 常见问题

-   Q：为什么基于MNS设计**logstash-input-oss**插件？

    A：因为OSS文件的变更需要有一种机制通知客户端，而目前OSS文件事件变更可以无缝的写入到MNS中。

-   Q：为什么不使用OSS的ListObjects API获取变更的文件？

    A：OSS在记录未处理的文件及已经处理的文件时会增加本地存储，当本地存储较大时，ListObjects API性能会降低。目前其他文件存储系统，如S3开源社区，也将ListObjects API改为了消息通知机制。


