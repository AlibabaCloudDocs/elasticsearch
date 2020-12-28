# Types of resources that can be authorized

This topic describes the types of Alibaba Cloud Logstash resources that can be authorized. You can grant different permissions on the resources to different users.

## General permission policies

Alibaba Cloud Logstash provides the following types of general permission policies. Select a policy based on your requirements.

-   `AliyunElasticsearchReadOnlyAccess`: the read-only permission on Elasticsearch or Logstash clusters. This policy can be attached to read-only users.
-   `AliyunElasticsearchFullAccess`: the management permission on Elasticsearch or Logstash clusters. This policy can be attached to administrators.

## Resource types and ARN formats

The following table lists the resource types supported by Logstash and the related Alibaba Cloud Resource Name \(ARN\) formats.

|Resource type|ARN format|
|-------------|----------|
|instances|`acs:elasticsearch:$regionId:$accountId:logstashes/*`|
|instances|`acs:elasticsearch:$regionId:$accountId:logstashes/$instanceId`|
|vpc|`acs:elasticsearch:$regionId:$accountId:vpc/*`|
|vswitch|`acs:elasticsearch:$regionId:$accountId:vswitch/*`|

-   `$regionId`: Set the value to the region ID of your Logstash cluster or to an asterisk \(`*`\).
-   `$accountId`: Set the value to the ID of your Alibaba Cloud account or to an asterisk \(`*`\).
-   `$instanceId`: Set the value to the ID of your Logstash cluster or to an asterisk \(`*`\).

## Permissions on Logstash clusters

**Note:** The following ARN formats are shortened. For information about the complete ARN formats, see [Resource types and ARN formats](#section_2s5_h3f_tki).

-   Manage clusters

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:CreateLogstash|Creates a cluster.|`logstashes/*` or `logstashes/$instanceId`|
    |elasticsearch:UpdateLogstash|Updates a cluster.|`logstashes/$instanceId`|
    |elasticsearch:ListLogstash|Queries clusters.|`logstashes/$instanceId`|
    |elasticsearch:UpdateLogstashDescription|Changes the name of a cluster.|`logstashes/$instanceId`|
    |elasticsearch:DescribeLogstash|Queries the details of a cluster.|`logstashes/$instanceId`|
    |elasticsearch:DeleteLogstash|Deletes a cluster.|`logstashes/$instanceId`|
    |elasticsearch:RestartLogstash|Restarts a cluster.|`logstashes/$instanceId`|
    |ListLogstashLog|Queries the logs of a cluster.|`logstashes/$instanceId`|
    |UpdateLogstashSettings|Modifies the YML file of a cluster.|`logstashes/$instanceId`|
    |UpdateLogstashChargeType|Changes the billing method of a cluster.|`logstashes/$instanceId`|
    |RenewLogstash|Renews a subscription cluster.|`logstashes/$instanceId`|

-   Manage plug-ins

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:ListPlugin|Queries plug-ins.|`logstashes/$instanceId`|
    |elasticsearch:InstallSystemPlugin|Installs a built-in plug-in.|`logstashes/$instanceId`|
    |elasticsearch:UninstallPlugin|Removes a plug-in.|`logstashes/$instanceId`|
    |elasticsearch:InstallUserPlugin|Installs a custom plug-in.|`logstashes/$instanceId`|

-   Query logs

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:ListLogstashLog|Queries logs.|`logstashes/$instanceId`|

-   Configure YML files

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:UpdateLogstashSettings|Configures a YML file.|`logstashes/$instanceId`|

-   Manage tasks

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:InterruptLogstashTask|Pauses a task.|`logstashes/$instanceId`|
    |elasticsearch:ResumeLogstashTask|Resumes a task.|`logstashes/$instanceId`|

-   Manage pipelines

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:UpdatePipelineManagementConfig|Updates the pipeline management method.|`logstashes/$instanceId`|
    |elasticsearch:DescribePipelineManagementConfig|Queries the pipeline management configuration.|`logstashes/$instanceId`|
    |elasticsearch:VerifyPipelineManagementConfig|Verifies the pipeline management configuration.|`logstashes/$instanceId`|
    |elasticsearch:ListAvailableEsInstanceIds|Queries valid cluster IDs.|`logstashes/$instanceId`|
    |elasticsearch:CreatePipelines|Creates a pipeline.|`logstashes/$instanceId`|
    |elasticsearch:UpdatePipelines|Modifies the configuration of a pipeline.|`logstashes/$instanceId`|
    |elasticsearch:RunPipelines|Deploys a pipeline immediately.|`logstashes/$instanceId`|
    |elasticsearch:StopPipelines|Stops a pipeline.|`logstashes/$instanceId`|
    |elasticsearch:ListPipeline|Queries pipelines.|`logstashes/$instanceId`|
    |elasticsearch:DescribePipeline|Queries the details of a pipeline.|`logstashes/$instanceId`|
    |elasticsearch:DeletePipelines|Deletes a pipeline.|`logstashes/$instanceId`|


## Regions supported by Logstash

|Region|Region ID|
|------|---------|
|China \(Hangzhou\)|cn-hangzhou|
|China \(Beijing\)|cn-beijing|
|China \(Shanghai\)|cn-shanghai|
|China \(Shenzhen\)|cn-shenzhen|
|China \(Qingdao\)|cn-qingdao|
|China \(Zhangjiakou\)|cn-zhangjiakou|

