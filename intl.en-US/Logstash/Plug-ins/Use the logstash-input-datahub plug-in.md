---
keyword: [logstash-input-datahub plug-in, Logstash data read from DataHub]
---

# Use the logstash-input-datahub plug-in

The logstash-input-datahub plug-in allows you to read data from DataHub to other data sources.

## Prerequisites

You have completed the following operations:

-   The logstash-input-datahub plug-in is installed.

    For more information, see [Install a Logstash plug-in]().

-   DataHub is activated. A project is created. A topic is created for the project. Data is imported to the topic.

    For more information, see [User Guide](https://help.aliyun.com/document_detail/158789.html).


## logstash-input-datahub application

After the prerequisites are met, you can create a pipeline task by following the instructions provided in [Use configuration files to manage pipelines](). For more information about the prerequisites, see [Prerequisites](#section_1zh_hzl_7sh). When you create a pipeline task, set the pipeline parameters by following the descriptions that are provided in the table of the Parameters section. After you set the parameters, you must save the settings and deploy the pipeline. Then, Alibaba Cloud Logstash is triggered to read data from DataHub to destination data sources.

The following example shows a configuration script. For more information about the parameters, see [Parameters](#section_ihp_ccv_ux0).

```
input {
    datahub {
        access_id => "Your accessId"
        access_key => "Your accessKey"
        endpoint => "Endpoint"
        project_name => "test_project"
        topic_name => "test_topic"
        interval=> 5
        #cursor => {
        #    "0"=>"20000000000000000000000003110091"
        #    "2"=>"20000000000000000000000003110091"
        #    "1"=>"20000000000000000000000003110091"
        #    "4"=>"20000000000000000000000003110091"
        #    "3"=>"20000000000000000000000003110000"
        #}
        shard_ids => []
        pos_file => "/home/admin/logstash/pos_file"
    }
}
output {
    file {
        path => "/home/admin/logstash/output"
    }
}
```

**Note:** Alibaba Cloud Logstash only supports data transmission within a Virtual Private Cloud \(VPC\). If source data is on the Internet, configure a NAT gateway to access Alibaba Cloud Logstash over the Internet. For more information, see [Configure a NAT gateway for data transmission over the Internet]().

## Parameters

The following table describes the parameters supported by **logstash-input-datahub**.

|Parameter|Data type|Required?|Description|
|---------|---------|---------|-----------|
|`endpoint`|String|Yes|The endpoint for accessing DataHub. For more information, see [DataHub domain names](https://help.aliyun.com/document_detail/158778.html).|
|`access_id`|String|Yes|The AccessKey ID of your account.|
|`access_key`|String|Yes|The AccessKey secret of your account.|
|`project_name`|String|Yes|The name of the DataHub project.|
|`topic_name`|String|Yes|The name of the DataHub topic.|
|`retry_times`|Number|No|The number of retries allowed. The value -1 indicates no limit. The value 0 indicates no retry. A value greater than 0 indicates that the specified number of retries are allowed. Default value: -1.|
|`retry_interval`|Number|No|The interval for retries. Unit: seconds.|
|`shard_ids`|Array|No|The list of shards that you want to consume. This parameter is empty by default. This indicates that all shards are consumed.|
|`cursor`|String|No|The start point of consumption. This parameter is empty by default. This indicates that shards are consumed from the start.|
|`pos_file`|String|Yes|The checkpoint file. This parameter is required. Checkpoints are preferentially used to resume consumption.|
|`enable_pb`|Boolean|No|Specifies whether to enable Protocol Buffers \(PB\) for transmission. Default value: true. If PB is not supported for transmission, set the value to false.|
|`compress_method`|String|No|The compression algorithm for data transmission. This parameter is not configured by default. Valid values: `lz4` and `deflate`.|
|`print_debug_info`|Boolean|No|Specifies whether to display the debug information of DataHub. Default value: false. If you set the value to true, a large amount of information is displayed. The information is used only for script debugging.|

