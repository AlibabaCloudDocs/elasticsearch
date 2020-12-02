# Resource types

This topic describes the resource types supported by Alibaba Cloud Logstash. You can refer to this topic to manage the permissions of your users.

## General permission policies

Alibaba Cloud Logstash provides the following types of general permission policies. Select a policy as required.

-   `AliyunElasticsearchReadOnlyAccess`: the read-only permissions on Elasticsearch or Logstash clusters. This policy can be granted to read-only users.
-   `AliyunElasticsearchFullAccess`: the management permissions on Elasticsearch or Logstash clusters. This policy can be granted to administrators.

## Resource types and ARNs

The following table lists the resource types supported by Logstash and their Alibaba Cloud Resource Names \(ARNs\).

|Resource type|ARN|
|-------------|---|
|instances|`acs:elasticsearch:$regionId:$accountId:logstashes/*`|
|instances|`acs:elasticsearch:$regionId:$accountId:logstashes/$instanceId`|
|vpc|`acs:elasticsearch:$regionId:$accountId:vpc/*`|
|vswitch|`acs:elasticsearch:$regionId:$accountId:vswitch/*`|

-   `$regionId`: Set the value to the region ID of the Logstash cluster or to an asterisk \(`*`\).
-   `$accountId`: Set the value to the ID of your Alibaba Cloud account or to an asterisk \(`*`\).
-   `$instanceId`: Set the value to the ID of the Logstash cluster or to an asterisk \(`*`\).

## Permissions on Logstash clusters

**Note:** The following ARNs are shortened. For information about their full names, see [Resource types and ARNs](#section_2s5_h3f_tki).

-   Manage instances

    |Action|Description|ARN|
    |------|-----------|---|
    |elasticsearch:CreateLogstash|Creates a cluster.|`logstashes/*`|
    |elasticsearch:ListLogstash|Queries clusters.|`logstashes/*`|
    |elasticsearch:UpdateLogstashDescription|Modifies the description of a cluster.|`logstashes/*` or `logstashes/$instanceId`|
    |elasticsearch:DescribeLogstash|Queries the description of a cluster.|`logstashes/*` or `logstashes/$instanceId`|
    |elasticsearch:DeleteLogstash|Deletes a cluster.|`logstashes/*` or `logstashes/$instanceId`|
    |elasticsearch:RestartLogstash|Restarts a cluster.|`logstashes/*` or `logstashes/$instanceId`|
    |elasticsearch:UpdateLogstash|Updates a cluster.|`logstashes/*` or `logstashes/$instanceId`|

-   Manage plug-ins

    |Action|Description|ARN|
    |------|-----------|---|
    |elasticsearch:ListPlugin|Queries plug-ins.|`logstashes/$instanceId`|
    |elasticsearch:InstallSystemPlugin|Installs a default plug-in.|`logstashes/$instanceId`|
    |elasticsearch:UninstallPlugin|Uninstalls a plug-in.|`logstashes/$instanceId`|
    |elasticsearch:InstallUserPlugin|Installs a custom plug-in.|`logstashes/$instanceId`|

-   Query logs

    |Action|Description|ARN|
    |------|-----------|---|
    |elasticsearch:ListLogstashLog|Queries logs.|`logstashes/$instanceId`|

-   Configure YML files

    |Action|Description|ARN|
    |------|-----------|---|
    |elasticsearch:UpdateLogstashSettings|Configures a YML file.|`logstashes/$instanceId`|

-   Pause tasks

    |Action|Description|ARN|
    |------|-----------|---|
    |elasticsearch:InterruptLogstashTask|Pauses a task.|`logstashes/$instanceId`|
    |elasticsearch:ResumeLogstashTask|Resumes a task.|`logstashes/$instanceId`|

-   Manage pipelines

    |Action|Description|ARN|
    |------|-----------|---|
    |elasticsearch:CreatePipelines|Creates a pipeline.|`logstashes/$instanceId`|
    |elasticsearch:UpdatePipelineManagementConfig|Specifies the pipeline management method.|`logstashes/$instanceId`|
    |elasticsearch:DescribePipelineManagementConfig|Obtains the pipeline management configuration.|`logstashes/$instanceId`|
    |elasticsearch:VerifyPipelineManagementConfig|Verifies the pipeline management configuration.|`logstashes/$instanceId`|
    |elasticsearch:ListAvailableEsInstanceIds|Obtains valid cluster IDs.|`logstashes/$instanceId`|
    |elasticsearch:DeletePipelines|Deletes a pipeline.|`logstashes/$instanceId`|
    |elasticsearch:UpdatePipelines|Modifies the configuration of a pipeline.|`logstashes/$instanceId`|
    |elasticsearch:RunPipelines|Deploys a pipeline immediately.|`logstashes/$instanceId`|
    |elasticsearch:StopPipelines|Stops a pipeline.|`logstashes/$instanceId`|


## Regions supported by Logstash

|Region|Region ID|
|------|---------|
|China \(Hangzhou\)|cn-hangzhou|
|China \(Beijing\)|cn-beijing|
|China \(Shanghai\)|cn-shanghai|
|China \(Shenzhen\)|cn-shenzhen|
|China \(Qingdao\)|cn-qingdao|
|China \(Zhangjiakou-Beijing Winter Olympic\)|cn-zhangjiakou|

