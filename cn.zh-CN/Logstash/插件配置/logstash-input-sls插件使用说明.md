---
keyword: [logstash-input-sls插件, logstash从日志服务获取日志]
---

# logstash-input-sls插件使用说明

logstash-input-sls插件是阿里云Logstash自带的默认插件。作为Logstash的input插件，logstash-input-sls插件提供了从日志服务获取日志的功能。

**说明：** logstash-input-sls是阿里云维护的开源插件，详情请参见[logstash-input-logservice](https://github.com/aliyun/logstash-input-logservice/blob/master/README.md)。

## 功能特性

-   支持分布式协同消费：可配置多台服务器同时消费一个Logstore服务。

    **说明：** 多台Logstash服务器进行分布式协同消费时，由于logstash-input-sls插件限制，需保证各个服务器仅部署一个input sls管道。如果单个服务器中存在多个input sls管道，输出端可能会出现数据重复的异常情况。

-   高性能：基于Java ConsumerGroup实现，单核消费速度可达20 MB/s。
-   高可靠：消费进度会被保存到服务端，宕机恢复时，会从上一次checkpoint处自动恢复。
-   自动负载均衡：根据消费者数量自动分配shard，消费者增加或退出后会自动进行负载均衡。

## 前提条件

您已完成以下操作：

-   安装logstash-input-sls插件。

    具体操作步骤请参见[安装Logstash插件](/cn.zh-CN/Logstash/插件配置/安装Logstash插件.md)。

-   创建日志服务项目和Logstore，并采集数据。

    具体操作步骤请参见[日志服务快速入门教程](/cn.zh-CN/.md)。


## 使用logstash-input-sls插件

满足以上[前提条件](#section_gsb_1ka_93k)后，您可以[通过配置文件管理管道](/cn.zh-CN/Logstash/管道任务管理/通过配置文件管理管道.md)的方式创建管道任务。在创建管道任务时，按照以下说明配置管道参数。配置完成后进行保存与部署，即可触发Logstash从日志服务获取日志。

以使用阿里云Logstash消费某一个Logstore，并将日志输出到阿里云Elasticsearch为例，配置示例如下。

```
input {
 logservice{
  endpoint => "your project endpoint"
  access_id => "your access id"
  access_key => "your access key"
  project => "your project name"
  logstore => "your logstore name"
  consumer_group => "consumer group name"
  consumer_name => "consumer name"
  position => "end"
  checkpoint_second => 30
  include_meta => true
  consumer_name_with_ip => true
  }
}

output {
  elasticsearch {
    hosts => ["http://es-cn-***.elasticsearch.aliyuncs.com:9200"]
    index => "<your_index>"
    user => "elastic"
    password => "changeme"
  }
}
```

假设某Logstore有10个shard，每个shard的数据流量1 MB/s；每台阿里云Logstash机器处理的能力为3 MB/s，可分配5台阿里云Logstash服务器，每个服务器创建一个input sls管道；并且每个服务器管道设置相同的`consumer_group`和`consumer_name`，将`consumer_name_with_ip`字段设置为`true`。

这种情况每台服务器会分配到2个shard，分别处理2 MB/s的数据。

## 参数说明

**logstash-input-sls**支持的参数如下。

|参数名|参数类型|是否必填|说明|
|---|----|----|--|
|`endpoint`|String|是|VPC网络下的日志服务项目的Endpoint，详情请参见[经典网络及VPC网络服务入口](/cn.zh-CN/开发指南/API 参考/服务入口.md)。|
|`access_id`|String|是|阿里云Access Key ID，需要具备ConsumerGroup相关权限，详情请参见[使用消费组消费](/cn.zh-CN/消费与投递/实时消费/消费组消费/通过消费组消费日志数据.md)。|
|`access_key`|String|是|阿里云Access Key Secret，需要具备ConsumerGroup相关权限，详情请参见[使用消费组消费](/cn.zh-CN/消费与投递/实时消费/消费组消费/通过消费组消费日志数据.md)。|
|`project`|String|是|日志服务项目名。|
|`logstore`|String|是|日志服务日志库名。|
|`consumer_group`|String|是|自定义消费组名。|
|`consumer_name`|String|是|自定义消费者名。同一个消费组内消费者名不能重复，否则会出现未定义行为。|
|`position`|String|是|消费位置，可选： -   `begin`：从日志库写入的第一条数据开始消费。
-   `end`：从当前时间点开始消费。
-   `yyyy-MM-dd HH:mm:ss`：从指定时间点开始消费。 |
|`checkpoint_second`|Number|否|每隔几秒checkpoint一次，建议10~60秒，不能低于10秒，默认30秒。|
|`include_meta`|Boolean|否|传入日志是否包含Meta，Meta包括日志source、time、tag以及topic，默认为`true`。|
|`consumer_name_with_ip`|Boolean|否|消费者名是否包含IP地址，默认为`true`。分布式协同消费下必须设置为`true`。|

## 性能基准测试信息

-   测试环境
    -   处理器：Intel\(R\) Xeon\(R\) Platinum 8163 CPU @ 2.50GHz，4 Core
    -   内存：8 GB
    -   环境：Linux
-   阿里云Logstash配置

    ```
    input {
      logservice{
      endpoint => "cn-hangzhou-intranet.log.aliyuncs.com"
      access_id => "***"
      access_key => "***"
      project => "test-project"
      logstore => "logstore1"
      consumer_group => "consumer_group1"
      consumer => "consumer1"
      position => "end"
      checkpoint_second => 30
      include_meta => true
      consumer_name_with_ip => true
      }
    }
    output {
      elasticsearch {
        hosts => ["http://es-cn-***.elasticsearch.aliyuncs.com:9200"]
        index => "myindex"
        user => "elastic"
        password => "changeme"
      }
    }
    ```

-   测试过程
    1.  使用Java Producer向Logstore发送数据，每秒分别发送2 MB、4 MB、8 MB、16 MB、32 MB数据。

        每条日志约500字节，包括10个Key和Value对。

    2.  启动阿里云Logstash消费Logstore中的数据，并确保消费延迟没有上涨（消费速度能够跟上生产的速度）。
-   测试结果

    |流量（MB/S）|CPU使用率（%）|内存占用量（GB）|
    |--------|---------|---------|
    |32|170.3|1.3|
    |16|83.3|1.3|
    |8|41.5|1.3|
    |4|21.0|1.3|
    |2|11.3|1.3|


