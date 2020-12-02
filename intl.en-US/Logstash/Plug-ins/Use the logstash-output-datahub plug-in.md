---
keyword: [logstash-output-datahub plug-in, Logstash batch data transfer to DataHub]
---

# Use the logstash-output-datahub plug-in

The logstash-output-datahub plug-in allows you to transfer data to DataHub.

## Prerequisites

You have completed the following operations:

-   The logstash-output-datahub plug-in is installed.

    For more information, see [Install a Logstash plug-in]().

-   DataHub is activated. A project is created. A topic is created for the project.

    For more information, see [User Guide](https://help.aliyun.com/document_detail/158789.html).

-   Input data sources are prepared.

    The input data sources can be selected from all the input plug-ins that are supported. For more information, see [Input plugins](https://www.elastic.co/guide/en/logstash/6.7/input-plugins.html).


## logstash-output-datahub application

After the prerequisites are met, you can create a pipeline task by following the instructions provided in [Use configuration files to manage pipelines](). For more information about the prerequisites, see [Prerequisites](#section_a43_2zx_hks). When you create a pipeline task, set the pipeline parameters by following the descriptions that are provided in the table of the Parameters section. After you set the parameters, you must save the settings and deploy the pipeline. Then, Alibaba Cloud Logstash is triggered to transfer data to DataHub.

This section provides an example on how to transfer a CSV file to DataHub. The CSV file contains the following data:

```
1111,1.23456789012E9,true,14321111111000000,string_dataxxx0,
2222,2.23456789012E9,false,14321111111000000,string_dataxxx1
```

The following table describes the format of the DataHub topic for the CSV file.

|Column name|Data type|
|-----------|---------|
|col1|BIGINT|
|col2|Double|
|col3|Boolean|
|col4|Timestamp|
|col5|String|

The following code shows the Logstash pipeline configuration. For more information about the parameters, see [Parameters](#section_4un_f2g_0rp).

```
input {
    file {
        path => "${APP_HOME}/data.csv"
        start_position => "beginning"
    }
}
filter{
    csv {
        columns => ['col1', 'col2', 'col3', 'col4', 'col5']
    }
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

For more information about how to transfer Log4j logs and JSON files, see [Logstash plug-ins](https://help.aliyun.com/document_detail/158836.html).

**Note:** Alibaba Cloud Logstash only supports data transmission within a Virtual Private Cloud \(VPC\). If source data is on the Internet, configure a NAT gateway to access Alibaba Cloud Logstash over the Internet. For more information, see [Configure a NAT gateway for data transmission over the Internet]().

## Parameters

The following table describes the parameters supported by **logstash-output-datahub**.

|Parameter|Data type|Required?|Description|
|---------|---------|---------|-----------|
|`endpoint`|String|Yes|The endpoint for accessing DataHub. For more information, see [DataHub domain names](https://help.aliyun.com/document_detail/158778.html).|
|`access_id`|String|Yes|The AccessKey ID of your account.|
|`access_key`|String|Yes|The AccessKey secret of your account.|
|`project_name`|String|Yes|The name of the DataHub project.|
|`topic_name`|String|Yes|The name of the DataHub topic.|
|`retry_times`|Number|No|The number of retries allowed. The value -1 indicates no limit. The value 0 indicates no retry. A value greater than 0 indicates that the specified number of retries are allowed. Default value: -1.|
|`retry_interval`|Number|No|The interval for retries. Unit: second. Default value: 5.|
|`skip_after_retry`|Boolean|No|Specifies whether to skip the upload of data in the current batch if the number of retries caused by a DataHub exception exceeds `retry_times`. Default value: false.|
|`approximate_request_bytes`|Number|No|The approximate number of bytes that can be sent in each request. Default value: 2048576, which equals 2 MB. This parameter is used to prevent a request from being rejected if the request body is too large.|
|`shard_keys`|Array|No|The names of fields. The plug-in uses the values of the fields to calculate hashes for writing data to a specific shard. **Note:** If `shard_keys` and `shard_ids` are not specified, the plug-in polls the shards to determine which shard is used to write data. |
|`shard_ids`|Array|No|The IDs of shards where the plug-in writes data. **Note:** If `shard_keys` and `shard_ids` are not specified, the plug-in polls the shards to determine which shard is used to write data. |
|`dirty_data_continue`|String|No|Specifies whether to skip dirty data during processing. Default value: false. If you set the value to true, you must specify `dirty_data_file`. The value true indicates that dirty data is skipped during data processing.|
|`dirty_data_file`|String|No|The name of the dirty data file. This parameter must be specified if the value of `dirty_data_continue` is set to true. **Note:** During data processing, the dirty data file is divided into part 1 and part 2. The raw dirty data is stored in part 1. The updated dirty data is stored in part 2. |
|`dirty_data_file_max_size`|Number|No|The maximum size of the dirty data file.|
|`enable_pb`|Boolean|No|Specifies whether to enable Protocol Buffers \(PB\) for transmission. Default value: true. If PB is not supported for transmission, set the value to false.|

