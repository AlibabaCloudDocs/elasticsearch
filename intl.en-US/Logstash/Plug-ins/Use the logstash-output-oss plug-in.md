---
keyword: [logstash-output-oss plug-in, transfer data to OSS]
---

# Use the logstash-output-oss plug-in

The logstash-output-oss plug-in allows you to transfer data to Object Storage Service \(OSS\).

**Note:** logstash-output-oss is an open source plug-in maintained by Alibaba Cloud. For more information, see [logstash-output-oss](https://github.com/aliyun/logstash-output-oss).

## Prerequisites

-   The logstash-output-oss plug-in is installed.

    For more information, see [Install a Logstash plug-in](/intl.en-US/Logstash/Plug-ins/Install a Logstash plug-in.md).

-   OSS is activated.

    For more information, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md).

-   A read/write OSS bucket is created. The AccessKey ID and AccessKey secret of your account that has write permissions on the OSS bucket are obtained.

    For more information, see [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md).

-   Data sources are prepared.

    The data sources can be selected from all the input plug-ins that are supported. For more information, see [Input plugins](https://www.elastic.co/guide/en/logstash/6.7/input-plugins.html).


## logstash-output-oss application

After the [prerequisites](#section_zfj_hlf_x98) are met, you can create a pipeline based on the instructions provided in [Use configuration files to manage pipelines](/intl.en-US/Logstash/Pipeline task management/Use configuration files to manage pipelines.md). When you create a pipeline, configure the pipeline parameters based on the descriptions that are provided in the table of the Parameters section. After you configure the parameters, you must save the settings and deploy the pipeline. Then, Alibaba Cloud Logstash is triggered to transfer data from data sources to OSS.

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

-   Alibaba Cloud Logstash supports data transmission only within a virtual private cloud \(VPC\). If source data is on the Internet, configure a NAT gateway to access Alibaba Cloud Logstash over the Internet. For more information, see [Configure a NAT gateway for data transmission over the Internet](/intl.en-US/Logstash/Network and security/Configure a NAT gateway for data transmission over the Internet.md).
-   For more information about how to use the logstash-output-oss plug-in, see [Synchronize OSS data based on event notifications from MNS]().

## Parameters

The following table describes the parameters supported by **logstash-output-oss**.

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|`endpoint`|String|Yes|The endpoint that is used to access OSS. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).|
|`bucket`|String|Yes|The name of the OSS bucket.|
|`access_key_id`|String|Yes|The AccessKey ID of your account that has write permissions on the OSS bucket.|
|`access_key_secret`|String|Yes|The AccessKey secret of your account that has write permissions on the OSS bucket.|
|`prefix`|String|No|The prefix of file names. If this parameter is not specified, it is empty by default. **Warning:** You can set this parameter to a string. In this case, many temporary files may be created on your on-premises machine. |
|`recover`|Boolean|No|Specifies whether to continue to upload the data on your on-premises machine if the program exits abnormally. Default value: true.|
|`additional_oss_settings`|Hash|No|Additional OSS client configurations. Valid values: -   `server_side_encryption_algorithm`: the server-side encryption method. Only AES-256 is supported.
-   `secure_connection_enabled`: specifies whether to enable HTTPS. Default value: false.
-   `max_connections_to_oss`: the maximum number of connections. Default value: 1024. |
|`temporary_directory`|String|Yes|The temporary directory that you use to cache data before the data is uploaded to OSS. Set the value to `/ssd/1/<Logstash cluster ID>/logstash/data/`. After the data transfer task is completed, the directory is deleted within seconds.|
|`rotation_strategy`|String|No|The file rotation strategy. Valid values: `size`, `time`, and `size_and_time`. Default value: size\_and\_time.|
|`size_rotate`|Number|No|If the size of a file is greater than or equal to the value of `size_rotate`, OSS rotates the file. This parameter is valid only when rotation\_strategy is set to size. Default value: 31457280. Unit: bytes.|
|`time_rotate`|Number|No|If the lifecycle of a file is greater than or equal to the value of `time_rotate`, OSS rotates the file. This parameter is valid only when rotation\_strategy is set to time. Default value: 15. Unit: minutes.|
|`upload_workers_count`|Number|No|The number of threads that are used to upload data at the same time.|
|`upload_queue_size`|Number|No|The size of the upload queue.|
|`encoding`|String|No|The method that is used to encode messages before you upload files to OSS. Both standard compression and GZIP compression are supported. Valid values: `gzip` and `none`. Default value: none.|

## Temporary files

When the logstash-output-oss plug-in transfers data to OSS, the plug-in creates a temporary file on your on-premises machine. Data is temporarily stored in this file. The logstash-output-oss plug-in transfers data to OSS on a regular basis. You can use the `temporary_directory` parameter to specify the path of the temporary file if you have specific requirements on the path.

The following example shows a temporary file path:

```
/ssd/1/<Logstash cluster ID>/logstash/data/eaced620-e972-0136-2a14-02b7449b****/logstash/1/ls.oss.eaced620-e972-0136-2a14-02b7449b****.2018-12-24T14.27.part-0.data
```

|Path element|Description|
|------------|-----------|
|/ssd/1/<Logstash cluster ID\>/logstash/data/|The temporary file directory specified by `temporary_directory`.|
|eaced620-e972-0136-2a14-02b7449b\*\*\*\*|The random Universally Unique Identifier \(UUID\).|
|logstash/1|The OSS object prefix.|
|ls.oss|Indicates that the temporary file is generated by the logstash-output-oss plug-in.|
|2018-12-24T14.27|The time when the temporary file was created.|
|part-0|The prefix of the temporary file.|
|.data|The file name extension. If `encoding` is set to `gzip`, the file name extension is `.gz`. Otherwise, the file name extension is `.data`.|

