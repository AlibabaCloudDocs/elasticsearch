---
keyword: [logstash-input-oss, Logstash OSS event notification, Logstash MNS]
---

# Use the logstash-input-oss plug-in

The logstash-input-oss plug-in is designed based on Alibaba Cloud Message Service \(MNS\). When an associated object in Object Storage Service \(OSS\) is updated, MNS sends a notification to the plug-in. After receiving the notification, the plug-in triggers Alibaba Cloud Logstash to read the latest data from OSS. You can configure event notification rules on the event notification page of OSS so that OSS can automatically send notifications to MNS when its object is updated.

**Note:** logstash-input-oss is an open-source plug-in maintained by Alibaba Cloud. For more information, see [logstash-input-oss](https://github.com/aliyun/logstash-input-oss).

## Precautions

-   When **logstash-input-oss** receives an MNS notification, Alibaba Cloud Logstash fully synchronizes the associated object.
-   If associated objects on OSS are text objects in the .gz or .gzip format, Alibaba Cloud Logstash processes the objects in the .gzip format. Objects in other formats are processed in the text format.
-   Objects are read in the text format. If an object is in a format that cannot be parsed, such as in the .jar or .bin format, the object may be read as garbled characters.

## Prerequisites

You have completed the following operations:

-   The logstash-input-oss plug-in is installed.

    For more information, see [Install a Logstash plug-in]().

-   OSS and MNS are activated, and the OSS buckets are in the same region as the MNS queues or topics.

    For more information, see [Activate OSS](/intl.en-US/Quick Start/Sign up for OSS.md) and [Activate MNS](https://www.alibabacloud.com/help/zh/doc-detail/27423.htm).

-   Event notification rules are configured in the OSS console.

    For more information, see [Configure event notification](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure event notification.md).


## logstash-input-oss application

After the prerequisites are met, you can create a pipeline task by following the instructions provided in [Use configuration files to manage pipelines](). For more information about the prerequisites, see [Prerequisites](#section_cdd_nhx_od6). When you create a pipeline task, set the pipeline parameters by following the descriptions that are provided in the table of the Parameters section. After you set the parameters, you must save the settings and deploy the pipeline. Then, Alibaba Cloud Logstash is triggered to retrieve data from OSS.

The following example shows how to retrieve data from OSS and write the data to Alibaba Cloud Elasticsearch:

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

**Note:** The MNS endpoint cannot be prefixed with http and must be an internal endpoint. Otherwise, an error is reported.

## Parameters

The following table describes the parameters supported by **logstash-input-oss**.

|Parameter|Data type|Required?|Description|
|---------|---------|---------|-----------|
|`endpoint`|String|Yes|The endpoint for accessing OSS. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).|
|`bucket`|String|Yes|The name of an OSS bucket.|
|`access_key_id`|String|Yes|The AccessKey ID of your account.|
|`access_key_secret`|String|Yes|The AccessKey secret of your account.|
|`prefix`|String|No|If this parameter is specified, the prefix of the names for the objects in the OSS bucket must match the value of this parameter. The prefix is not a regular expression.|
|`additional_oss_settings`|Hash|No|Additional OSS client configurations. Valid values: `secure_connection_enabled` and `max_connections_to_oss`.|
|`delete`|Boolean|No|Specifies whether to delete processed objects from the original OSS bucket. Default value: `false`.|
|`backup_to_bucket`|String|No|The name of the OSS bucket that stores the backups of processed objects.|
|`backup_to_dir`|String|No|The local directory that stores the backups of processed files.|
|`backup_add_prefix`|String|No|The prefix of a key after an object is processed. The key is a full path that includes the object name in OSS. If you back up data to the same or another OSS bucket, this parameter allows you to choose a new folder to store objects.|
|`include_object_properties`|Boolean|No|Specifies whether to include the properties of an OSS object in `[@metadata][oss]`. The properties refer to last\_modified, content\_type, and metadata. If this parameter is not specified, `[@metadata][oss][key]` always exists.|
|`exclude_pattern`|String|No|The Ruby-based regular expression of the key that you want to exclude from an OSS bucket.|
|`mns_settings`|Hash|Yes|The configurations of MNS. Valid values:

-   `endpoint`: the endpoint for accessing MNS. The endpoint cannot be prefixed with http and must be an internal endpoint. Otherwise, an error is reported.
-   `queue`: the name of an MNS queue.
-   `poll_interval_seconds`: the maximum waiting time for a ReceiveMessage request when the queue contains no messages. Default value: 10s.
-   `wait_seconds`: the maximum polling waiting time for a ReceiveMessage request. Unit: seconds.

 For more information about ReceiveMessage, see [ReceiveMessage](https://www.alibabacloud.com/help/zh/doc-detail/35136.html). |

## FAQ

-   Q: Why is the **logstash-input-oss** plug-in designed based on MNS?

    A: A mechanism is required to notify clients of OSS object updates. The events about OSS object updates can be seamlessly written to MNS.

-   Q: Why is the ListObjects API operation of OSS not used to obtain updated objects?

    A: OSS consumes more local storage when it records unprocessed and processed objects. When the local storage consumption is high, the performance of ListObjects decreases. Other file storage systems, such as the Amazon S3 open-source community, also replace ListObjects with the message notification mechanism.


