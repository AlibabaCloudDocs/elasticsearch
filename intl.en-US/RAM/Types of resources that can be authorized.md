---
keyword: [types of Elasticsearch resources that can be authorized, API operations that can be specified in the Action element]
---

# Types of resources that can be authorized

When you use a permission policy to grant permissions to a RAM user, you must configure the Action and Resource elements in the policy. This topic describes the types and Alibaba Cloud Resource Name \(ARN\) formats of the resources that are related to Alibaba Cloud Elasticsearch. These resources include Elasticsearch resources, Logstash resources, tags, virtual private clouds \(VPCs\), vSwitches, Cloud Monitor resources, and resources for intelligent O&M. This topic also describes the API operations that you can specify in the Action element.

## Resource types and ARN formats

-   Elasticsearch

    |Resource type|ARN format|
    |-------------|----------|
    |instances|`acs:elasticsearch:$regionId:$accountId:instances/*`|
    |instances|`acs:elasticsearch:$regionId:$accountId:instances/$instanceId`|
    |tags|`acs:elasticsearch:$regionId:$accountId:tags/*`|

-   Logstash

    |Resource type|ARN format|
    |-------------|----------|
    |instances|`acs:elasticsearch:$regionId:$accountId:logstashes/*`|
    |instances|`acs:elasticsearch:$regionId:$accountId:logstashes/$instanceId`|

-   VPC and vSwitch

    |Resource type|ARN format|
    |-------------|----------|
    |vpc|`acs:elasticsearch:$regionId:$accountId:vpc/*`|
    |vswitch|`acs:elasticsearch:$regionId:$accountId:vswitch/*`|

    -   `$regionId`: Set the value to the region ID of your Elasticsearch cluster or to an asterisk \(`*`\). The following table lists the IDs of all regions where Elasticsearch is available.

        |Country/District|Region|Region ID|
        |----------------|------|---------|
        |China|China \(Shanghai\)|cn-shanghai|
        |China \(Shenzhen\)|cn-shenzhen|
        |China \(Qingdao\)|cn-qingdao|
        |China \(Zhangjiakou\)|cn-zhangjiakou|
        |China \(Beijing\)|cn-beijing|
        |China \(Hangzhou\)|cn-hangzhou|
        |China \(Hong Kong\)|cn-hongkong|
        |Asia Pacific|Singapore \(Singapore\)|ap-southeast-1|
        |Malaysia \(Kuala Lumpur\)|ap-southeast-3|
        |Japan \(Tokyo\)|ap-northeast-1|
        |Australia \(Sydney\)|ap-southeast-2|
        |Indonesia \(Jakarta\)|ap-southeast-5|
        |Europe & Americas|US \(Virginia\)|us-east-1|
        |US \(Silicon Valley\)|us-west-1|
        |Germany \(Frankfurt\)|eu-central-1|
        |UK \(London\)|eu-west-1|
        |Middle East & India|India \(Mumbai\)|ap-south-1|

    -   `$accountId`: Set the value to the ID of your Alibaba Cloud account or to an asterisk \(`*`\).
    -   `$instanceId`: Set the value to the ID of your cluster or to an asterisk \(`*`\).

## Permissions on Elasticsearch resources

**Note:** The ARN formats in the following table are shortened. For information about the complete ARN formats, see [Resource types and ARN formats](#section_bw0_lnk_iuj).

-   Manage Elasticsearch clusters

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:CreateInstance|Creates a cluster.|`instances/*`|
    |elasticsearch:ListInstance|Queries clusters.|`instances/*`|
    |elasticsearch:DescribeInstance|Queries the description of a cluster.|`instances/*` or `instances/$instanceId`|
    |elasticsearch:DeleteInstance|Deletes a cluster.|`instances/*` or `instances/$instanceId`|
    |elasticsearch:RestartInstance|Restarts a cluster.|`instances/*` or `instances/$instanceId`|
    |elasticsearch:UpdateInstance|Updates a cluster.|`instances/*` or `instances/$instanceId`|
    |elasticsearch:UpdateInstanceSettings|Modifies the YML file of a cluster.|`instances/$instanceId`|
    |elasticsearch:UpdateDescription|Changes the name of a cluster.|`instances/$instanceId`|
    |elasticsearch:UpdateAdminPwd|Changes the password that is used to access a cluster.|`instances/$instanceId`|
    |elasticsearch:ListSearchLogs|Queries the logs of a cluster.|`instances/$instanceId`|
    |elasticsearch:DowngradeInstance|Scales in a cluster.|`instances/$instanceId`|
    |elasticsearch:CancelTask|Cancels a data migration task of a cluster.|`instances/$instanceId`|
    |elasticsearch:UpdateKibanaSettings|Modifies the configuration of the Kibana node in a cluster.|`instances/$instanceId`|
    |elasticsearch:DescribeKibanaSettings|Queries the configuration of the Kibana node in a cluster.|`instances/$instanceId`|
    |elasticsearch:DescribeElasticsearchHealth|Queries the health status of a cluster.|`instances/$instanceId`|
    |elasticsearch:DeactivateZones|Performs a switchover for nodes in a zone.|`instances/$instanceId`|
    |elasticsearch:ActivateZones|Performs a recovery for nodes on which a switchover is performed.|`instances/$instanceId`|
    |elasticsearch:MigrateToOtherZone|Migrates nodes in a zone.|`instances/$instanceId`|
    |elasticsearch:ResumeElasticsearchTask|Resumes a task for a cluster.|`instances/$instanceId`|
    |elasticsearch:InterruptElasticsearchTask|Pauses a task for a cluster.|`instances/$instanceId`|
    |elasticsearch:UpdateAdvancedSetting|Modifies the configuration of the garbage collector for a cluster.|`instances/$instanceId`|
    |elasticsearch:ListPipelineIds|Queries Logstash pipelines.|`instances/$instanceId`|
    |elasticsearch:UpdateInstanceChargeType|Changes the billing method of a cluster.|`instances/$instanceId`|
    |elasticsearch:RenewInstance|Renews a subscription cluster.|`instances/$instanceId`|
    |elasticsearch:UpgradeInstanceEngineVersion|Upgrades the version of a cluster.|`instances/$instanceId`|
    |elasticsearch:ModifyInstanceMaintainTime|Modifies the maintenance window of a cluster.|`instances/$instanceId`|

-   Manage plug-ins

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:ListPlugin|Queries plug-ins.|`instances/$instanceId`|
    |elasticsearch:InstallSystemPlugin|Installs a built-in plug-in.|`instances/$instanceId`|
    |elasticsearch:UninstallPlugin|Removes a plug-in.|`instances/$instanceId`|

-   Manage access over a network

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:UpdatePublicNetwork|Enables or disables the Public Network Access feature for a cluster.|`instances/$instanceId`|
    |elasticsearch:UpdatePublicIps|Modifies the public IP address whitelist of a cluster.|`instances/$instanceId`|
    |elasticsearch:UpdateWhiteIps|Modifies the private IP address whitelist of a cluster.|`instances/$instanceId`|
    |elasticsearch:UpdateKibanaIps|Modifies the IP address whitelist for access to the Kibana console of a cluster.|`instances/$instanceId`|
    |elasticsearch:OpenHttps|Enables HTTPS.|`instances/$instanceId`|
    |elasticsearch:CloseHttps|Disables HTTPS.|`instances/$instanceId`|
    |elasticsearch:DescribeConnectableClusters|Queries the clusters that can be connected in a VPC.|`instances/$instanceId`|
    |elasticsearch:ListConnectedClusters|Queries the clusters that are connected.|`instances/$instanceId`|
    |elasticsearch:AddConnectableCluster|Connects clusters.|`instances/$instanceId`|
    |elasticsearch:DeleteConnectedCluster|Disconnects the clusters that are connected.|`instances/$instanceId`|
    |elasticsearch:ModifyWhiteIps|Modifies the whitelists of a cluster, including the IP address whitelist for access to the Kibana console of the cluster.|`instances/$instanceId`|
    |elasticsearch:TriggerNetwork|Enables or disables the Public Network Access or Private Network Access feature for Elasticsearch or Kibana.|`instances/$instanceId`|

    **Note:** If you specify a whitelist update-related operation such as UpdatePublicIps, UpdateWhiteIps, or UpdateKibanaIps for the Action element in a policy, you must also specify ModifyWhiteIps.

-   Manage dictionaries

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:UpdateDict|Modifies IK and synonym dictionaries.|`instances/$instanceId`|


## Permissions on tags

|Action|Description|ARN format|
|------|-----------|----------|
|elasticsearch:ListTags|Queries tags.|`tags/$instanceId`|
|elasticsearch:CreateTags|Creates or updates a tag.|`tags/$instanceId`|
|elasticsearch:RemoveTags|Removes a tag.|`tags/$instanceId`|

## Permissions on Cloud Monitor

**Note:** The ARN formats in the following table are shortened by using asterisks \(`*`\).

|Action|Description|ARN format|
|------|-----------|----------|
|cms:ListProductOfActiveAlert|Queries the services for which Cloud Monitor is activated.|`*`|
|cms:ListAlarm|Queries the settings of a specific or all alert rules.|`*`|
|cms:QueryMetricList|Queries the monitoring data of a cluster over a specific period of time.|`*`|

## Permissions on VPCs and vSwitches displayed on the Elasticsearch buy page

|Action|Description|ARN format|
|------|-----------|----------|
|DescribeVpcs|Queries VPCs.|`vpc/*`|
|DescribeVswitches|Queries vSwitches.|`vswitch/*`|

## Permissions on intelligent O&M

|Action|Description|ARN format|
|------|-----------|----------|
|elasticsearch:OpenDiagnosis|Enables intelligent health diagnostics.|`instances/*` or `instances/$instanceId`|
|elasticsearch:CloseDiagnosis|Disables intelligent health diagnostics.|`instances/*` or `instances/$instanceId`|
|elasticsearch:UpdateDiagnosisSettings|Updates health diagnostic settings.|`instances/*` or `instances/$instanceId`|
|elasticsearch:DescribeDiagnosisSettings|Queries health diagnostic settings.|`instances/*` or `instances/$instanceId`|
|elasticsearch:ListInstanceIndices|Queries cluster indexes.|`instances/*` or `instances/$instanceId`|
|elasticsearch:DiagnoseInstance|Starts intelligent health diagnostics.|`instances/*` or `instances/$instanceId`|
|elasticsearch:ListDiagnoseReportIds|Queries diagnostic report IDs.|`instances/*` or `instances/$instanceId`|
|elasticsearch:DescribeDiagnoseReport|Queries the details of a diagnostic report.|`instances/*` or `instances/$instanceId`|
|elasticsearch:ListDiagnoseReport|Queries the details of diagnostic reports.|`instances/*` or `instances/$instanceId`|

## Permissions on Logstash resources

-   Manage Logstash clusters

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

-   Manage logs

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:ListLogstashLog|Queries logs.|`logstashes/$instanceId`|

-   Manage YML files

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
    |elasticsearch:DescribePipelineManagementConfig|Queries the pipeline management configurations.|`logstashes/$instanceId`|
    |elasticsearch:VerifyPipelineManagementConfig|Verifies the pipeline management configurations.|`logstashes/$instanceId`|
    |elasticsearch:ListAvailableEsInstanceIds|Queries valid cluster IDs.|`logstashes/$instanceId`|
    |elasticsearch:CreatePipelines|Creates a pipeline.|`logstashes/$instanceId`|
    |elasticsearch:UpdatePipelines|Modifies the configuration of a pipeline.|`logstashes/$instanceId`|
    |elasticsearch:RunPipelines|Deploys a pipeline immediately.|`logstashes/$instanceId`|
    |elasticsearch:StopPipelines|Stops a pipeline.|`logstashes/$instanceId`|
    |elasticsearch:ListPipeline|Queries pipelines.|`logstashes/$instanceId`|
    |elasticsearch:DescribePipeline|Queries the details of a pipeline.|`logstashes/$instanceId`|
    |elasticsearch:DeletePipelines|Deletes a pipeline.|`logstashes/$instanceId`|


## Additional information

When you create custom policies, you can use the resources and API operations provided in this topic. For more information, see [Create a custom policy](/intl.en-US/RAM/Create a custom policy.md).

