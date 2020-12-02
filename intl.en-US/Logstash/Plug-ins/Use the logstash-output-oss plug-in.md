---
keyword: [logstash-output-oss plug-in, Logstash data transfer to OSS]
---

# Use the logstash-output-oss plug-in

The logstash-output-oss plug-in allows you to transfer data to Object Storage Service \(OSS\).

**Note:** logstash-output-oss is an open-source plug-in maintained by Alibaba Cloud. For more information, see [logstash-output-oss](https://github.com/aliyun/logstash-output-oss).

## Prerequisites

You have completed the following operations:

-   The logstash-output-oss plug-in is installed.

    For more information, see [Install a Logstash plug-in]().

-   OSS is activated.

    For more information, see [Activate OSS](/intl.en-US/Quick Start/Sign up for OSS.md).

-   A read/write OSS bucket is created. The AccessKey ID and AccessKey secret of your account that has write permissions on the OSS bucket are obtained.

    For more information, see [Create buckets](/intl.en-US/Quick Start/Create buckets.md).

-   Input data sources are prepared.

    The input data sources can be selected from all the input plug-ins that are supported. For more information, see [Input plugins](https://www.elastic.co/guide/en/logstash/6.7/input-plugins.html).


## logstash-output-oss application

After the prerequisites are met, you can create a pipeline task by following the instructions provided in [Use configuration files to manage pipelines](). For more information about the prerequisites, see [Prerequisites](#section_zfj_hlf_x98). When you create a pipeline task, set the pipeline parameters by following the descriptions that are provided in the table of the Parameters section. After you set the parameters, you must save the settings and deploy the pipeline. Then, Alibaba Cloud Logstash is triggered to transfer data to OSS.

The following example shows how to transfer data from Beats to OSS:

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
    temporary_directory => "/ssd/1/<Logstash cluster ID>/logstash/data/22"                            
    encoding => "gzip"                                 
    additional_oss_settings => {
      max_connections_to_oss => 1024                      
      secure_connection_enabled => false                  
    }
  }
}
```

**Note:**

Alibaba Cloud Logstash only supports data transmission within a Virtual Private Cloud \(VPC\). If source data is on the Internet, configure a NAT gateway to access Alibaba Cloud Logstash over the Internet. For more information, see [Configure a NAT gateway for data transmission over the Internet]().

## Parameters

The following table describes the parameters supported by **logstash-output-oss**.

|Parameter|Data type|Required?|Description|
|---------|---------|---------|-----------|
|`endpoint`|String|Yes|The endpoint for accessing OSS. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).|
|`bucket`|String|Yes|The name of the OSS bucket.|
|`access_key_id`|String|Yes|The AccessKey ID of your account that has write permissions on the OSS bucket.|
|`access_key_secret`|String|Yes|The AccessKey secret of your account that has write permissions on the OSS bucket.|
|`prefix`|String|No|The prefix of file names. If this parameter is not specified, it is empty by default. **Warning:** The value allows for strings, and many local temporary files can be created. |
|`recover`|Boolean|No|Specifies whether the upload of local data can continue if the program exits abnormally. Default value: true.|
|`additional_oss_settings`|Hash|No|Additional OSS client configurations. Valid values: -   `server_side_encryption_algorithm`: the server-side encryption method. Only AES-256 is supported.
-   `secure_connection_enabled`: specifies whether to enable HTTPS. Default value: false.
-   `max_connections_to_oss`: the maximum number of connections. Default value: 1024. |
|`temporary_directory`|String|Yes|The temporary directory that you use to cache data before it is uploaded to OSS. Set the value to `/ssd/1/<Logstash cluster ID>/logstash/data/`.|
|`rotation_strategy`|String|No|The file rotation strategy. Valid values: `size`, `time`, and `size_and_time`. Default value: size\_and\_time.|
|`size_rotate`|Number|No|If the size of a file is greater than or equal to the value of `size_rotate`, OSS rotates the file. This parameter takes effect when rotation\_strategy is set to size. Default value: 15. Unit: minutes.|
|`time_rotate`|Number|No|If the lifecycle of a file is greater than or equal to the value of `time_rotate`, OSS rotates the file. This parameter takes effect when rotation\_strategy is set to time. Default value: 31457280. Unit: bytes.|
|`upload_workers_count`|Number|No|The number of concurrent upload threads.|
|`upload_queue_size`|Number|No|The size of the upload queue.|
|`encoding`|String|No|The method for encoding messages before you upload files to OSS. Both standard compression and GZIP compression are supported. Valid values: `gzip` and `none`. Default value: none.|

## Temporary files

When the logstash-output-oss plug-in transfers data to OSS, the plug-in creates a local temporary file in Logstash. Data is temporarily stored in this file. The logstash-output-oss plug-in transfers data to OSS at regular intervals. You can use the `temporary_directory` parameter to specify the path of the temporary file if you have specific requirements on the path.

The following example shows a temporary file path:

```
/ssd/1/<Logstash cluster ID>/logstash/data/eaced620-e972-0136-2a14-02b7449b****/logstash/1/ls.oss.eaced620-e972-0136-2a14-02b7449b****.2018-12-24T14.27.part-0.data
```

|Path element|Description|
|------------|-----------|
|/ssd/1/<Logstash cluster ID\>/logstash/data/|The temporary file directory specified by `temporary_directory`.|
|eaced620-e972-0136-2a14-02b7449b\*\*\*\*|The random Universally Unique Identifier \(UUID\).|
|logstash/1|The OSS object prefix.|
|ls.oss|The OSS output plug-in.|
|2018-12-24T14.27|The time when the current file was created.|
|part-0|The Nth file that uses the specific prefix.|
|.data|The suffix of the output data file. If `encoding` is set to `gzip`, the suffix is `.gz`. Otherwise, the suffix is `.data`.|

