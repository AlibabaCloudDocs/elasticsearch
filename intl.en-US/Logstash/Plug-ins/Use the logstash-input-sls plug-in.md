---
keyword: [logstash-input-sls plug-in, retrieve logs from Log Service]
---

# Use the logstash-input-sls plug-in

logstash-input-sls is a built-in input plug-in of Alibaba Cloud Logstash. This plug-in retrieves logs from Log Service.

**Note:** logstash-input-sls is an open source plug-in maintained by Alibaba Cloud. For more information, see [logstash-input-logservice](https://github.com/aliyun/logstash-input-logservice/blob/master/README.md).

## Features

-   Distributed collaborative consumption: Multiple servers can be configured to consume log data in a Logstore at the same time.

    **Note:** If you want to use multiple Logstash servers to implement distributed collaborative consumption, make sure that only one pipeline with logstash-input-sls installed is deployed on each server. This is because of the limits on the logstash-input-sls plug-in. If multiple pipelines with logstash-input-sls installed are deployed on a single server, duplicated data may exist in the output.

-   High performance: If you use a Java consumer group, the consumption speed of a single-core CPU can reach 20 MB/s.
-   High reliability: logstash-input-sls saves the consumption progress on servers. If a server is recovered from an exception, it can continue to consume data from the last consumption checkpoint.
-   Automatic load balancing: Shards are automatically allocated based on the number of consumers in a consumer group. If consumers join or leave the consumer group, logstash-input-sls automatically re-allocates the shards.

## Operation



-   The logstash-input-sls plug-in is installed.

    For more information, see [Install a Logstash plug-in](/intl.en-US/Logstash/Plug-ins/Install a Logstash plug-in.md).

-   A Log Service project and a Logstore are created. Data is collected.

    For more information, see [Quick start](/intl.en-US/.md).


## logstash-input-sls application

After the [prerequisites](#section_gsb_1ka_93k) are met, you can create a pipeline based on the instructions provided in [Use configuration files to manage pipelines](/intl.en-US/Logstash/Pipeline task management/Use configuration files to manage pipelines.md). When you create a pipeline, configure the pipeline parameters based on the descriptions that are provided in the table of the Parameters section. After you configure the parameters, you must save the settings and deploy the pipeline. Then, Alibaba Cloud Logstash is triggered to retrieve data from Log Service.

The following example shows how to use Alibaba Cloud Logstash to consume log data in a Logstore and transfer the data to Alibaba Cloud Elasticsearch:

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

Make the following assumptions: The Logstore has 10 shards. The data traffic of each shard is 1 MB/s. The processing capacity of each Logstash server is 3 MB/s. Five Logstash servers are allocated, and one pipeline with logstash-input-sls installed is deployed on each server. Each server uses the same settings of `consumer_group` and `consumer_name`. `consumer_name_with_ip` is set to `true`.

In this case, two shards are allocated to each server, and each shard processes data at 2 MB/s.

## Parameters

The following table describes the parameters supported by **logstash-input-sls**.

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|`endpoint`|String|Yes|The endpoint of the Log Service project in a virtual private cloud \(VPC\). For more information, see [Endpoints for the classic network and VPC](/intl.en-US/Developer Guide/API Reference/Endpoints.md).|
|`access_id`|String|Yes|The AccessKey ID of your account that has permissions on the consumer group. For more information, see [Use consumer groups to consume logs](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md).|
|`access_key`|String|Yes|The AccessKey secret of your account that has permissions on the consumer group. For more information, see [Use consumer groups to consume logs](/intl.en-US/Log consumption and shipping/Real-time subscription and consumption/Consumption by consumer groups/Use consumer groups to consume log data.md).|
|`project`|String|Yes|The name of the Log Service project.|
|`logstore`|String|Yes|The name of the Logstore.|
|`consumer_group`|String|Yes|The name of the consumer group, which can be customized.|
|`consumer_name`|String|Yes|The name of a consumer, which can be customized. The name of each consumer must be unique in a consumer group. Otherwise, undefined behavior may occur.|
|`position`|String|Yes|The start position to consume log data. Valid values: -   `begin`: Data is consumed from the first log entry that is written to the Logstore.
-   `end`: Data is consumed from the current time.
-   `yyyy-MM-dd HH:mm:ss`: Data is consumed from the specified time. |
|`checkpoint_second`|Number|No|The interval for triggering a checkpoint. We recommend that you set the interval to a value in the range of 10s to 60s. Minimum value: 10. Default value: 30. Unit: seconds.|
|`include_meta`|Boolean|No|Specifies whether input logs contain metadata, such as the log source, time, tag, and topic. Default value: `true`.|
|`consumer_name_with_ip`|Boolean|No|Specifies whether the consumer name contains an IP address. Default value: `true`. For distributed collaborative consumption, set the value to `true`.|

## Performance benchmark test

-   Test environment
    -   CPU: Intel\(R\) Xeon\(R\) Platinum 8163 processor@2.50 GHz, 4-core
    -   Memory: 8 GB
    -   Operating system: Linux
-   Configuration of a Logstash pipeline

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

-   Test procedure
    1.  Use a Java producer to send 2 MB, 4 MB, 8 MB, 16 MB, and 32 MB of data to the Logstore every second.

        Each log entry contains 10 key-value pairs and is about 500 bytes in length.

    2.  Start Logstash to consume data in the Logstore. Make sure that the consumption latency does not increase and the consumption speed can keep pace with the production speed.
-   Test results

    |Traffic \(MB/s\)|CPU utilization \(%\)|Memory usage \(GB\)|
    |----------------|---------------------|-------------------|
    |32|170.3|1.3|
    |16|83.3|1.3|
    |8|41.5|1.3|
    |4|21.0|1.3|
    |2|11.3|1.3|


