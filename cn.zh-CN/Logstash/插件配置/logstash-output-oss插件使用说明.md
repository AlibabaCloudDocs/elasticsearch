---
keyword: [logstash-output-oss插件, logstash批量传送数据到oss]
---

# logstash-output-oss插件使用说明

通过logstash-output-oss插件，您可以将数据批量传送到阿里云对象存储服务OSS（Object Storage Service）中。本文介绍如何使用logstash-output-oss插件。

**说明：** logstash-output-oss是阿里云维护的开源插件，详细信息，请参见[logstash-output-oss](https://github.com/aliyun/logstash-output-oss)。

## 前提条件

您已完成以下操作：

-   安装logstash-output-oss插件。

    具体操作，请参见[安装logstash-output-oss插件](/cn.zh-CN/Logstash/插件配置/安装Logstash插件.md)。

-   开通阿里云OSS服务。

    具体操作，请参见[开通阿里云OSS服务](/cn.zh-CN/控制台用户指南/开通OSS服务.md)。

-   创建可读写的OSS Bucket，并且获取拥有该Bucket写权限的Accesskey ID和Accesskey Secret。

    具体操作，请参见[创建可读写的OSS Bucket](/cn.zh-CN/快速入门/控制台快速入门/创建存储空间.md)。

-   准备输入数据源。

    输入数据源可以为input支持的所有输入源插件中的数据，详细信息，请参见[input插件](https://www.elastic.co/guide/en/logstash/6.7/input-plugins.html)。


## 使用logstash-output-oss插件

满足以上[前提条件](#section_zfj_hlf_x98)后，您可以[通过配置文件管理管道](/cn.zh-CN/Logstash/管道任务管理/通过配置文件管理管道.md)的方式创建管道任务。在创建管道任务时，按照以下说明配置Pipeline参数，保存并部署后，即可触发阿里云Logstash向OSS传送数据。

以将Beats采集文件中的数据传送到OSS为例。

```
input {
  beats {
    port => "8044"
    codec => json {
      charset => "UTF-8"

  }
}
output {
   oss {
    endpoint => "http://oss-cn-hangzhou-internal.aliyuncs.com"              
    bucket => "zl-log-output-test"                          
    access_key_id => "LTAIaxxxxx******"                 
    access_key_secret => "zuxxxx8hBpXs3e6i******"         
    prefix => "oss/database"                         
    recover => true                                      
    rotation_strategy => "size_and_time"                  
    time_rotate => 1                                     
    size_rotate => 1000
    temporary_directory => "/ssd/1/<Logstash实例ID>/logstash/data/22"                            
    encoding => "gzip"                                 
    additional_oss_settings => {
      max_connections_to_oss => 1024                      
      secure_connection_enabled => false                  
    }
    codec => json {
      charset => "UTF-8"
    }
  }
}
```

**说明：**

-   阿里云Logstash目前只支持在同一专有网络下进行数据传输，如果源端数据在公网下，请参见[配置NAT公网数据传输](/cn.zh-CN/Logstash/网络与安全/配置NAT公网数据传输.md)，在公网环境下进行数据传输。
-   logstash-output-oss插件的具体应用，请参见[基于MNS事件通知迁移OSS数据](/cn.zh-CN/最佳实践/存储产品数据迁移/基于MNS事件通知迁移OSS数据.md)。

## 参数说明

**logstash-output-oss**插件支持的参数如下。

|参数|类型|是否必选|说明|
|--|--|----|--|
|endpoint|string|是|OSS对外服务的访问域名。详细信息，请参见[访问域名和数据中心](/cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。|
|bucket|string|是|OSS的Bucket名称。|
|access\_key\_id|string|是|拥有对应Bucket写权限的Accesskey ID。|
|access\_key\_secret|string|是|拥有对应Bucket写权限的Accesskey Secret。|
|prefix|string|否|指定文件名前缀，不指定默认为空。 **警告：** 此选项支持字符串，因此可能会创建很多临时本地文件。 |
|recover|Boolean|否|程序出现异常退出时，保存在本地的数据是否可以继续上传。默认为true。|
|additional\_oss\_settings|hash|否|附加的OSS客户端配置。可选值： -   server\_side\_encryption\_algorithm：服务端加密方式，只支持AES256。
-   secure\_connection\_enabled：是否开启https，默认false。
-   max\_connections\_to\_oss：最大连接数，默认1024。 |
|temporary\_directory|string|是|数据上传到OSS之前的临时目录路径定义，必须设置为/ssd/1/<Logstash实例ID\>/logstash/data/。任务结束后，一般会在秒级被删除。|
|rotation\_strategy|string|否|文件滚动更新策略。可选值：size、time、size\_and\_time（默认）。|
|size\_rotate|number|否|如果文件大小大于等于size\_rotate，OSS将滚动更新文件（依赖rotation\_strategy）。默认为31457280 Bytes。|
|time\_rotate|number|否|如果文件的生存时长大于等于time\_rotate，OSS将滚动更新文件（依赖rotation\_strategy）。默认为15分钟。|
|upload\_workers\_count|number|否|上传线程并发数。|
|upload\_queue\_size|number|否|上载队列大小。|
|encoding|string|否|消息在上传文件到OSS之前，支持纯压缩和gzip压缩。可选值：gzip、none（默认）。|

## 临时文件说明

logstash-output-oss在传送数据到OSS时，会在Logstash本地创建一个临时文件。数据临时存储在该文件下，logstash-output-oss插件定期推送数据到OSS。可通过设置temporary\_directory参数，设置该临时文件的地址。如果您对输出数据保存的路径有要求，可以设置该临时文件路径。

临时文件路径示例如下。

```
/ssd/1/<Logstash实例ID>/logstash/data/eaced620-e972-0136-2a14-02b7449b****/logstash/1/ls.oss.eaced620-e972-0136-2a14-02b7449b****.2018-12-24T14.27.part-0.data
```

|路径|说明|
|--|--|
|/ssd/1/<Logstash实例ID\>/logstash/data/|由temporary\_directory指定的临时目录。|
|eaced620-e972-0136-2a14-02b7449b\*\*\*\*|随机UUID。|
|logstash/1|OSS对象前缀。|
|ls.oss|临时文件，表示由logstash-output-oss插件生成。|
|2018-12-24T14.27|临时文件创建的时间。|
|part-0|临时文件的前缀。|
|.data|临时文件的后缀。如果设置`encoding`为`gzip`，将会以`.gz`结尾，其他以`.data`结尾。|

