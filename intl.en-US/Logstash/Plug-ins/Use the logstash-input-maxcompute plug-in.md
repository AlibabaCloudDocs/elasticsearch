---
keyword: [logstash-input-maxcompute plug-in, data read from the offline tables of MaxCompute]
---

# Use the logstash-input-maxcompute plug-in

The logstash-input-maxcompute plug-in allows you to read data from the offline tables of MaxCompute to other data sources.

## Prerequisites

You have completed the following operations:

-   The logstash-input-maxcompute plug-in is installed.

    For more information, see [Install a Logstash plug-in]().

-   Alibaba Cloud MaxCompute is activated. A project is created. A table is created for the project. Data is imported to the table.

    For more information, see [Prepare](/intl.en-US/Prepare/Activate MaxCompute.md) and [Quick Start](/intl.en-US/Quick Start/Create and view a table.md).


## logstash-input-maxcompute application

After the prerequisites are met, you can create a pipeline task by following the instructions provided in [Use configuration files to manage pipelines](). For more information about the prerequisites, see [Prerequisites](#section_zaa_2nz_yyc). When you create a pipeline task, set the pipeline parameters by following the descriptions that are provided in the table of the Parameters section. After you set the parameters, you must save the settings and deploy the pipeline. Then, Alibaba Cloud Logstash is triggered to read data from MaxCompute to destination data sources.

The following example shows a configuration script. For more information about the parameters, see [Parameters](#section_k6m_0se_d35).

```
input {
    maxcompute {
        access_id => "Your accessId"
        access_key => "Your accessKey"
        endpoint => "maxcompute service endpoint"
        project_name => "Your project"
        table_name => "Your table name"
        partition => "pt='p1',dt='d1'"
        thread_num => 1
        dirty_data_file => "/ssd/1/tmp/xxxxx"
    }
}

output {
    stdout {
        codec => rubydebug
    }
}
```

**Note:**

-   Alibaba Cloud Logstash only supports data transmission within a Virtual Private Cloud \(VPC\). If source data is on the Internet, configure a NAT gateway to access Alibaba Cloud Logstash over the Internet. For more information, see [Configure a NAT gateway for data transmission over the Internet]().
-   **logstash-input-maxcompute** fully synchronizes data to destination data sources.

## Parameters

The following table describes the parameters supported by **logstash-input-maxcompute**.

|Parameter|Data type|Required?|Description|
|---------|---------|---------|-----------|
|`endpoint`|String|Yes|The endpoint for accessing MaxCompute. For more information, see [Regions where MaxCompute is available and corresponding endpoints](/intl.en-US/Prepare/Configure endpoints.md).|
|`access_id`|String|Yes|The AccessKey ID of your account.|
|`access_key`|String|Yes|The AccessKey secret of your account.|
|`project_name`|String|Yes|The name of the MaxCompute project.|
|`table_name`|String|Yes|The name of the MaxCompute table.|
|`partition`|String|Yes|The partition field. The MaxCompute table is partitioned by using this field. Example: `sale_date='201911'` and `region='hangzhou'`.|
|`thread_num`|Number|Yes|The number of threads. Default value: 1.|
|`retry_interval`|Number|No|The interval for retries. Unit: seconds.|
|`dirty_data_file`|String|Yes|The file for recording logs about processing failures.|

