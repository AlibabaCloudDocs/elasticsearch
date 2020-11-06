---
keyword: [types of Elasticsearch resources that can be authorized, actions for authorizing Elasticsearch resources]
---

# Types of resources that can be authorized

This topic describes the types of Elasticsearch resources that can be authorized. You can grant different permissions on the resources to different users.

## Resource types and ARN formats

The following table lists the resource types supported by Elasticsearch and the related Alibaba Cloud Resource Name \(ARN\) formats.

|Resource type|ARN format|
|-------------|----------|
|instances|`acs:elasticsearch:$regionId:$accountId:instances/*`|
|instances|`acs:elasticsearch:$regionId:$accountId:instances/$instanceId`|
|vpc|`acs:elasticsearch:$regionId:$accountId:vpc/*`|
|vswitch|`acs:elasticsearch:$regionId:$accountId:vswitch/*`|
|tags|`acs:elasticsearch:$regionId:$accountId:tags/*`|

-   `$regionId`: Set the value to the region ID of your Elasticsearch cluster or to an asterisk \(`*`\).
-   `$accountId`: Set the value to the ID of your Alibaba Cloud account or to an asterisk \(`*`\).
-   `$instanceId`: Set the value to the ID of your Elasticsearch cluster or to an asterisk \(`*`\).

For more information about how to grant permissions on Elasticsearch resources, see [Create a custom policy](/intl.en-US/RAM/Create a custom policy.md).

## Permissions on Elasticsearch resources

**Note:** The following ARN formats are shortened. For information about the complete ARN formats, see [Resource types and ARN formats](#section_bw0_lnk_iuj).

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
    |elasticsearch:UpdateWhiteIps|Modifies the VPC whitelist of a cluster.|`instances/$instanceId`|
    |elasticsearch:UpdateKibanaIps|Modifies the Kibana whitelist of a cluster.|`instances/$instanceId`|
    |elasticsearch:OpenHttps|Enables HTTPS.|`instances/$instanceId`|
    |elasticsearch:CloseHttps|Disables HTTPS.|`instances/$instanceId`|
    |elasticsearch:DescribeConnectableClusters|Queries the clusters that can be connected in a VPC.|`instances/$instanceId`|
    |elasticsearch:ListConnectedClusters|Queries clusters that are connected.|`instances/$instanceId`|
    |elasticsearch:AddConnectableCluster|Connects clusters.|`instances/$instanceId`|
    |elasticsearch:DeleteConnectedCluster|Disconnects clusters.|`instances/$instanceId`|
    |elasticsearch:ModifyWhiteIps|Modifies the whitelists \(including the Kibana whitelist\) of a cluster.|`instances/$instanceId`|
    |elasticsearch:TriggerNetwork|Enables or disables the Public Network Access or Private Network Access feature for Elasticsearch or Kibana.|`instances/$instanceId`|

    **Note:** If you specify a whitelist update-related operation such as UpdatePublicIps, UpdateWhiteIps, or UpdateKibanaIps for the Action parameter in a policy, you must also specify ModifyWhiteIps.

-   Manage dictionaries

    |Action|Description|ARN format|
    |------|-----------|----------|
    |elasticsearch:UpdateDict|Modifies IK and synonym dictionaries.|`instances/$instanceId`|


## Permissions on tags

|Action|Description|ARN format|
|------|-----------|----------|
|elasticsearch:ListTags|Allows RAM users to query tags.|`tags/$instanceId`|
|elasticsearch:CreateTags|Allows RAM users to create or update a tag.|`tags/$instanceId`|
|elasticsearch:RemoveTags|Allows RAM users to remove a tag.|`tags/$instanceId`|

For more information about how to create a custom policy for tags, see [Grant permissions on tags to a RAM user](/intl.en-US/RAM/Grant permissions on tags to a RAM user.md).

## Permissions on Cloud Monitor

**Note:** The following ARN formats are shortened by using an asterisk \(`*`\).

|Action|Description|ARN format|
|------|-----------|----------|
|cms:ListProductOfActiveAlert|Queries services with Cloud Monitor activated.|`*`|
|cms:ListAlarm|Queries the settings of a specific or all alert rules.|`*`|
|cms:QueryMetricList|Queries the metric data of a cluster over a period of time.|`*`|

## Permissions on VPCs and VSwitches on the Elasticsearch buy page

**Note:** The following ARN formats are shortened. For information about the complete ARN formats, see [Resource types and ARN formats](#section_bw0_lnk_iuj).

|Action|Description|ARN format|
|------|-----------|----------|
|DescribeVpcs|Queries VPCs.|`vpc/*`|
|DescribeVswitches|Queries VSwitches.|`vswitch/*`|

## Permissions on intelligent O&M

**Note:** The following ARN formats are shortened. For information about the complete ARN formats, see [Resource types and ARN formats](#section_bw0_lnk_iuj).

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
|elasticsearch:ListDiagnoseReport|Queries diagnostic reports.|`instances/*` or `instances/$instanceId`|

## Supported regions

|Country/District|Region|Region ID|
|----------------|------|---------|
|China|China \(Shanghai\)|cn-shanghai|
|China \(Shenzhen\)|cn-shenzhen|
|China \(Qingdao\)|cn-qingdao|
|China \(Zhangjiakou-Beijing Winter Olympics\)|cn-zhangjiakou|
|China \(Beijing\)|cn-beijing|
|China \(Hangzhou\)|cn-hangzhou|
|China \(Hong Kong\)|cn-hongkong|
|Asia Pacific|Singapore \(Singapore\)|ap-southeast-1|
|Malaysia \(Kuala Lumpur\)|ap-southeast-3|
|Japan \(Tokyo\)|ap-northeast-1|
|Australia \(Sydney\)|ap-southeast-2|
|Indonesia \(Jakarta\)|ap-southeast-5|
|Europe & Americas|US \(Silicon Valley\)|us-west-1|
|Germany \(Frankfurt\)|eu-central-1|
|US \(Virginia\)|us-east-1|
|Middle East & India|India \(Mumbai\)|ap-south-1|

