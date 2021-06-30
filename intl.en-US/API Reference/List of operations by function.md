# List of operations by function

The following tables list API operations available for use in Elasticsearch, Kibana, Logstash, and Beats. If you want to use an API operation that is not listed in these tables, contact Alibaba Cloud technical support engineers to obtain it.

## Elasticsearch

|Category|Operation|Description|
|--------|---------|-----------|
|Cluster management|[createInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/createInstance.md)|Creates an Elasticsearch cluster.|
|[ListInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/ListInstance.md)|Queries the detailed information of all Elasticsearch clusters or a specified Elasticsearch cluster.|
|[DescribeInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/DescribeInstance.md)|Queries the detailed information of a specified Elasticsearch cluster.|
|[EstimatedRestartTime](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/EstimatedRestartTime.md)|Obtains the estimated time that is required to restart an Elasticsearch cluster.|
|[RestartInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/RestartInstance.md)|Restarts a specified Elasticsearch cluster.|
|[UpdateInstanceChargeType](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/UpdateInstanceChargeType.md)|Switches the billing method of an Elasticsearch cluster from pay-as-you-go to subscription.|
|[UpdateDescription](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/UpdateDescription.md)|Changes the name of a specified Elasticsearch cluster.|
|[DeleteInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/DeleteInstance.md)|Releases a specified pay-as-you-go Elasticsearch cluster. After the cluster is released, the physical resources used by the cluster are reclaimed. The data stored on the cluster is deleted and cannot be recovered. The disks attached to the nodes in the cluster and the snapshots created for the cluster are released.|
|[CancelDeletion](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/CancelDeletion.md)|Restores an Elasticsearch cluster that is frozen after it is released.|
|[RenewInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/RenewInstance.md)|Renews a subscription Elasticsearch cluster.|
|[ActivateZones](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/ActivateZones.md)|Restores the nodes in a disabled zone. This operation is available only for multi-zone Elasticsearch clusters.|
|[DeactivateZones](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/DeactivateZones.md)|Disables one or more zones where a multi-zone Elasticsearch cluster resides and migrates the nodes in the disabled zones to other zones.|
|[DescribeRegions](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/DescribeRegions.md)|Queries the regions whether Elasticsearch is available.|
|[InterruptElasticsearchTask](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/InterruptElasticsearchTask.md)|Suspends a change task of an Elasticsearch cluster. After the task is suspended, the cluster is in the suspended state. This operation is available only for Elasticsearch clusters in the activating state.|
|[ResumeElasticsearchTask](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/ResumeElasticsearchTask.md)|Resumes a change task of an Elasticsearch cluster.|
|[ListAllNode](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/ListAllNode.md)|Queries the information of all the nodes in an Elasticsearch cluster.|
|[DescribeElasticsearchHealth](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/DescribeElasticsearchHealth.md)|Queries the health status of a specified Elasticsearch cluster.|
|[ListInstanceIndices](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/ListInstanceIndices.md)|Queries the indexes stored on an Elasticsearch cluster.|
|[MigrateToOtherZone](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/MigrateToOtherZone.md)|Migrates the nodes in a zone to another zone.|
|[MoveResourceGroup](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/MoveResourceGroup.md)|Migrates an Elasticsearch cluster to a specified resource group.|
|[ModifyInstanceMaintainTime](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/ModifyInstanceMaintainTime.md)|Enables and modifies the maintenance window of an Elasticsearch cluster.|
|[GetRegionConfiguration](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/GetRegionConfiguration.md)|Queries the configuration information in the current region. This operation returns all the configuration information in the current region. The information is for reference only. The actual information in the console and on the buy page prevails.|
|Tag management|[ListTags](/intl.en-US/API Reference/Elasticsearch instances/Manage cluster tags/ListTags.md)|Queries all the visible user tags.|
|[ListTagResources](/intl.en-US/API Reference/Elasticsearch instances/Manage cluster tags/ListTagResources.md)|Queries the tags that are added to one or more resources.|
|[TagResources](/intl.en-US/API Reference/Elasticsearch instances/Manage cluster tags/TagResources.md)|Adds tags to resources.|
|[UntagResources](/intl.en-US/API Reference/Elasticsearch instances/Manage cluster tags/UntagResources.md)|Removes tags from resources.|
|Data migration|[GetTransferableNodes](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/GetTransferableNodes.md)|Queries the nodes on which data can be migrated based on the specified node type and number of nodes.|
|[ValidateTransferableNodes](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/ValidateTransferableNodes.md)|Checks whether the data on specific nodes in a specified Elasticsearch cluster can be migrated.|
|[TransferNode](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/TransferNode.md)|Runs a data migration task.|
|[ListDataTasks](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/ListDataTasks.md)|Queries the information of a data migration task.|
|[CreateDataTasks](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/CreateDataTasks.md)|Creates a data migration task to migrate data from a specified Elasticsearch cluster to the current Elasticsearch cluster.|
|[GetClusterDataInformation](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/GetClusterDataInformation.md)|Queries the data information of an Elasticsearch cluster.|
|[DeleteDataTask](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/DeleteDataTask.md)|Deletes a data migration task.|
|[CancelTask](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/CancelTask.md)|Cancels a data migration task.|
|Cluster configuration upgrade and downgrade|[GetSuggestShrinkableNodes](/intl.en-US/API Reference/Elasticsearch instances/Upgrade or downgrade a cluster/GetSuggestShrinkableNodes.md)|Queries the nodes that can be removed from an Elasticsearch cluster based on the specified node type and number of nodes.|
|[ValidateShrinkNodes](/intl.en-US/API Reference/Elasticsearch instances/Upgrade or downgrade a cluster/ValidateShrinkNodes.md)|Checks whether specific nodes can be removed from a specified Elasticsearch cluster.|
|[ShrinkNode](/intl.en-US/API Reference/Elasticsearch instances/Upgrade or downgrade a cluster/ShrinkNode.md)|Removes nodes from an Elasticsearch cluster.|
|[UpgradeEngineVersion](/intl.en-US/API Reference/Elasticsearch instances/Upgrade or downgrade a cluster/UpgradeEngineVersion.md)|Upgrades the version or kernel of an Elasticsearch cluster. You can upgrade Elasticsearch clusters from V5.5.3 to V5.6.16, from V5.6.16 to V6.3.2, or from V6.3.2 to V6.7.0.|
|[UpdateInstance](/intl.en-US/API Reference/Elasticsearch instances/Upgrade or downgrade a cluster/UpdateInstance.md)|Upgrades or downgrades the configuration of an Elasticsearch cluster.|
|Cluster configuration|[UpdateInstanceSettings](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/UpdateInstanceSettings.md)|Updates the configuration in the YML file of a specified Elasticsearch cluster.|
|[UpdateHotIkDicts](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/UpdateHotIkDicts.md)|Performs a rolling update for the IK dictionaries of an Elasticsearch cluster.|
|[UpdateSynonymsDicts](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/UpdateSynonymsDicts.md)|Updates the synonym dictionary of an Elasticsearch cluster.|
|[UpdateDict](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/UpdateDict.md)|Performs a standard update for the IK dictionaries of an Elasticsearch cluster.|
|[UpdateAliwsDict](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/UpdateAliwsDict.md)|Updates the dictionary file of the analysis-aliws plug-in. This plug-in allows you to upload a tailored dictionary file to it.|
|[ListDictInformation](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/ListDictInformation.md)|Queries and verifies the details of the dictionary object stored in Object Storage Service \(OSS\) when you add the object to an Elasticsearch cluster.|
|[UpdateAdvancedSetting](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/UpdateAdvancedSetting.md)|Updates the garbage collector \(GC\) configuration of a specified Elasticsearch cluster.|
|Plug-in management|[ListPlugins](/intl.en-US/API Reference/Elasticsearch instances/Manage plug-ins/ListPlugins.md)|Queries the plug-ins that are installed on a specified Elasticsearch cluster.|
|[InstallSystemPlugin](/intl.en-US/API Reference/Elasticsearch instances/Manage plug-ins/InstallSystemPlugin.md)|Installs a built-in plug-in.|
|[UninstallPlugin](/intl.en-US/API Reference/Elasticsearch instances/Manage plug-ins/UninstallPlugin.md)|Uninstalls a built-in plug-in.|
|[InstallUserPlugins](/intl.en-US/API Reference/Elasticsearch instances/Manage plug-ins/InstallUserPlugins.md)|Installs a custom plug-in that is uploaded to the Elasticsearch console.|
|Log query|[ListSearchLog](/intl.en-US/API Reference/Elasticsearch instances/Query logs/ListSearchLog.md)|Queries the logs of an Elasticsearch cluster.|
|Security configuration|[TriggerNetwork](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/TriggerNetwork.md)|Enables or disables the Public Network Access or Private Network Access feature for Elasticsearch or Kibana.|
|[UpdatePrivateNetworkWhiteIps](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/UpdatePrivateNetworkWhiteIps.md)|Updates the private IP address whitelist of a specified Elasticsearch cluster.|
|[UpdatePublicWhiteIps](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/UpdatePublicWhiteIps.md)|Updates the public IP address whitelist of a specified Elasticsearch cluster.|
|[UpdatePublicNetwork](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/UpdatePublicNetwork.md)|Enables or disables the Public Network Access feature for a specified Elasticsearch cluster.|
|[UpdateWhiteIps](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/UpdateWhiteIps.md)|Updates the private IP address whitelist of a specified Elasticsearch cluster.|
|[ModifyWhiteIps](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/ModifyWhiteIps.md)|Updates the IP address whitelist of a specified Elasticsearch cluster.|
|[UpdateAdminPassword](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/UpdateAdminPassword.md)|Updates the password for the elastic account of a specified Elasticsearch cluster.|
|[OpenHttps](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/OpenHttps.md)|Enables HTTPS. Before you call this operation, make sure that your Elasticsearch cluster contains client nodes.|
|[CloseHttps](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/CloseHttps.md)|Disables HTTPS.|
|[AddConnectableCluster](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/AddConnectableCluster.md)|Connects Elasticsearch clusters.|
|[DeleteConnectedCluster](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/DeleteConnectedCluster.md)|Disconnects Elasticsearch clusters.|
|[DescribeConnectableClusters](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/DescribeConnectableClusters.md)|Queries the Elasticsearch clusters that can be connected to a specified Elasticsearch cluster. The Elasticsearch clusters that are connected to the specified Elasticsearch cluster are excluded.|
|[ListConnectedClusters](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/ListConnectedClusters.md)|Queries the Elasticsearch clusters that are connected to a specified Elasticsearch cluster.|
|Data backup|[CreateSnapshot](/intl.en-US/API Reference/Elasticsearch instances/Backup data/CreateSnapshot.md)|Creates a snapshot in an Elasticsearch cluster.|
|[DescribeSnapshotSetting](/intl.en-US/API Reference/Elasticsearch instances/Backup data/DescribeSnapshotSetting.md)|Queries the data backup configuration of an Elasticsearch cluster.|
|[UpdateSnapshotSetting](/intl.en-US/API Reference/Elasticsearch instances/Backup data/UpdateSnapshotSetting.md)|Updates the data backup configuration of a specified Elasticsearch cluster.|
|[ListSnapshotReposByInstanceId](/intl.en-US/API Reference/Elasticsearch instances/Backup data/ListSnapshotReposByInstanceId.md)|Queries the shared OSS repositories configured for an Elasticsearch cluster.|
|[ListAlternativeSnapshotRepos](/intl.en-US/API Reference/Elasticsearch instances/Backup data/ListAlternativeSnapshotRepos.md)|Queries the shared OSS repositories that can be configured for an Elasticsearch cluster.|
|[AddSnapshotRepo](/intl.en-US/API Reference/Elasticsearch instances/Backup data/AddSnapshotRepo.md)|Creates a shared OSS repository for an Elasticsearch cluster.|
|[DeleteSnapshotRepo](/intl.en-US/API Reference/Elasticsearch instances/Backup data/DeleteSnapshotRepo.md)|Deletes a shared OSS repository for an Elasticsearch cluster.|
|Intelligent O&M|[OpenDiagnosis](/intl.en-US/API Reference/Elasticsearch instances/Intelligent maintenance/OpenDiagnosis.md)|Enables the intelligent O&M feature for an Elasticsearch cluster.|
|[CloseDiagnosis](/intl.en-US/API Reference/Elasticsearch instances/Intelligent maintenance/CloseDiagnosis.md)|Disables the intelligent O&M feature for an Elasticsearch cluster.|
|[DiagnoseInstance](/intl.en-US/API Reference/Elasticsearch instances/Intelligent maintenance/DiagnoseInstance.md)|Diagnoses an Elasticsearch cluster.|
|[ListDiagnoseReport](/intl.en-US/API Reference/Elasticsearch instances/Intelligent maintenance/ListDiagnoseReport.md)|Queries the historical intelligent O&M reports of an Elasticsearch cluster.|
|[ListDiagnoseReportIds](/intl.en-US/API Reference/Elasticsearch instances/Intelligent maintenance/ListDiagnoseReportIds.md)|Queries the IDs of the historical intelligent O&M reports of an Elasticsearch cluster.|
|[ListDiagnoseIndices](/intl.en-US/API Reference/Elasticsearch instances/Intelligent maintenance/ListDiagnoseIndices.md)|Queries the indexes for health diagnosis performed on a specified Elasticsearch cluster.|
|[DescribeDiagnoseReport](/intl.en-US/API Reference/Elasticsearch instances/Intelligent maintenance/DescribeDiagnoseReport.md)|Queries a historical intelligent O&M report.|
|[DescribeDiagnosisSettings](/intl.en-US/API Reference/Elasticsearch instances/Intelligent maintenance/DescribeDiagnosisSettings.md)|Queries the scenario settings for intelligent O&M of an Elasticsearch cluster.|
|[UpdateDiagnosisSettings](/intl.en-US/API Reference/Elasticsearch instances/Intelligent maintenance/UpdateDiagnosisSettings.md)|Updates the scenario settings for intelligent O&M of an Elasticsearch cluster.|

## Kibana

|Operation|Description|
|---------|-----------|
|[DescribeKibanaSettings](/intl.en-US/API Reference/Kibana/DescribeKibanaSettings.md)|Queries the configuration of Kibana.|
|[UpdateKibanaSettings](/intl.en-US/API Reference/Kibana/UpdateKibanaSettings.md)|Modifies the configuration of Kibana. You can change only the language settings of Kibana.|
|[ListKibanaPlugins](/intl.en-US/API Reference/Kibana/ListKibanaPlugins.md)|Queries the plug-ins of Kibana.|
|[InstallKibanaSystemPlugin](/intl.en-US/API Reference/Kibana/InstallKibanaSystemPlugin.md)|Installs a built-in plug-in for Kibana. Before you call this operation, make sure that the specifications of your Kibana node are 2 vCPUs and 4 GiB of memory or higher.|
|[UninstallKibanaPlugin](/intl.en-US/API Reference/Kibana/UninstallKibanaPlugin.md)|Uninstalls a plug-in for Kibana.|
|[UpdateKibanaWhiteIps](/intl.en-US/API Reference/Kibana/UpdateKibanaWhiteIps.md)|Updates the IP address whitelist for access to the Kibana console of a specified Elasticsearch cluster.|

## Logstash

|Category|Operation|Description|
|--------|---------|-----------|
|Cluster management|[CreateLogstash](/intl.en-US/API Reference/Logstash/Manage clusters/CreateLogstash.md)|Creates a Logstash cluster.|
|[ListLogstash](/intl.en-US/API Reference/Logstash/Manage clusters/ListLogstash.md)|Queries the detailed information of all Logstash clusters or a specified Logstash cluster.|
|[DescribeLogstash](/intl.en-US/API Reference/Logstash/Manage clusters/DescribeLogstash.md)|Queries the detailed information of a specified Logstash cluster.|
|[UpdateLogstash](/intl.en-US/API Reference/Logstash/Manage clusters/UpdateLogstash.md)|Modifies the configuration of a specified Logstash cluster, such as the name, quota, disk size, and number of nodes.|
|[RenewLogstash](/intl.en-US/API Reference/Logstash/Manage clusters/RenewLogstash.md)|Renews a Logstash cluster.|
|[RestartLogstash](/intl.en-US/API Reference/Logstash/Manage clusters/RestartLogstash.md)|Restarts a specified Logstash cluster. After the cluster is restarted, it is in the activating state.|
|[UpdateLogstashDescription](/intl.en-US/API Reference/Logstash/Manage clusters/UpdateLogstashDescription.md)|Changes the name of a specified Logstash cluster.|
|[UpdateLogstashChargeType](/intl.en-US/API Reference/Logstash/Manage clusters/UpdateLogstashChargeType.md)|Changes the billing method of a Logstash cluster from pay-as-you-go to subscription.|
|[EstimatedLogstashRestartTime](/intl.en-US/API Reference/Logstash/Manage clusters/EstimatedLogstashRestartTime.md)|Queries the estimated time that is required to restart a Logstash cluster.|
|[DeleteLogstash](/intl.en-US/API Reference/Logstash/Manage clusters/DeleteLogstash.md)|Releases a Logstash cluster.|
|[CancelLogstashDeletion](/intl.en-US/API Reference/Logstash/Manage clusters/CancelLogstashDeletion.md)|Restores a Logstash cluster that is frozen after it is released.|
|Cluster configuration|[UpdateLogstashSettings](/intl.en-US/API Reference/Logstash/Configure clusters/UpdateLogstashSettings.md)|Updates the configuration of a specified Logstash cluster.|
|[ListExtendfiles](/intl.en-US/API Reference/Logstash/Configure clusters/ListExtendfiles.md)|Queries the third-party libraries of a Logstash cluster.|
|[UpdateExtendfiles](/intl.en-US/API Reference/Logstash/Configure clusters/UpdateExtendfiles.md)|Updates the third-party libraries of a Logstash cluster.|
|Cluster monitoring|[ListAvailableEsInstanceIds](/intl.en-US/API Reference/Logstash/Monitoring cluster/ListAvailableEsInstanceIds.md)|Queries the Elasticsearch clusters that can be associated with a Logstash cluster when you configure the X-Pack Monitoring feature for the Logstash cluster.|
|[ValidateConnection](/intl.en-US/API Reference/Logstash/Monitoring cluster/ValidateConnection.md)|Tests the connectivity between a Logstash cluster and its associated Elasticsearch cluster when you configure monitoring and alerting for the Logstash cluster.|
|[UpdateXpackMonitorConfig](/intl.en-US/API Reference/Logstash/Monitoring cluster/UpdateXpackMonitorConfig.md)|Updates the X-Pack monitoring and alerting configuration of a Logstash cluster.|
|[DescribeXpackMonitorConfig](/intl.en-US/API Reference/Logstash/Monitoring cluster/DescribeXpackMonitorConfig.md)|Queries the X-Pack monitoring and alerting configuration of a Logstash cluster.|
|Plug-in management|[ListLogstashPlugins](/intl.en-US/API Reference/Logstash/Manage plug-ins/ListLogstashPlugins.md)|Queries the detailed information of all plug-ins or a specified plug-in.|
|[InstallLogstashSystemPlugin](/intl.en-US/API Reference/Logstash/Manage plug-ins/InstallLogstashSystemPlugin.md)|Installs a plug-in.|
|[UninstallLogstashPlugin](/intl.en-US/API Reference/Logstash/Manage plug-ins/UninstallLogstashPlugin.md)|Uninstalls a plug-in.|
|Log query|[ListLogstashLog](/intl.en-US/API Reference/Logstash/Query logs/ListLogstashLog.md)|Queries the logs of a Logstash cluster.|
|Change task management|[InterruptLogstashTask](/intl.en-US/API Reference/Logstash/Manage cluster task/InterruptLogstashTask.md)|Suspends a change task of a Logstash cluster. After the task is suspended, the Logstash cluster is in the suspended state.|
|[ResumeLogstashTask](/intl.en-US/API Reference/Logstash/Manage cluster task/ResumeLogstashTask.md)|Resumes a change task of a Logstash cluster. After the task is resumed, the Logstash cluster is in the activating state.|
|Pipeline management|[CreatePipelines](/intl.en-US/API Reference/Logstash/Manage pipelines/CreatePipelines.md)|Creates a pipeline in a Logstash cluster.|
|[ListPipeline](/intl.en-US/API Reference/Logstash/Manage pipelines/ListPipeline.md)|Queries the pipelines of a Logstash cluster.|
|[DescribePipeline](/intl.en-US/API Reference/Logstash/Manage pipelines/DescribePipeline.md)|Queries the detailed information of a pipeline in a Logstash cluster.|
|[UpdatePipelines](/intl.en-US/API Reference/Logstash/Manage pipelines/UpdatePipelines.md)|Updates the information of pipelines in a Logstash cluster.|
|[RunPipelines](/intl.en-US/API Reference/Logstash/Manage pipelines/RunPipelines.md)|Runs pipelines in a Logstash cluster.|
|[StopPipelines](/intl.en-US/API Reference/Logstash/Manage pipelines/StopPipelines.md)|Stops pipelines in a Logstash cluster.|
|[UpdatePipelineManagementConfig](/intl.en-US/API Reference/Logstash/Manage pipelines/UpdatePipelineManagementConfig.md)|Updates the management method of pipelines in a Logstash cluster.|
|[DescribePipelineManagementConfig](/intl.en-US/API Reference/Logstash/Manage pipelines/DescribePipelineManagementConfig.md)|Queries the management configuration of pipelines in a Logstash cluster.|
|[DeletePipelines](/intl.en-US/API Reference/Logstash/Manage pipelines/DeletePipelines.md)|Deletes a specified pipeline in a Logstash cluster.|

## Beats

|Operation|Description|
|---------|-----------|
|[CreateCollector](/intl.en-US/API Reference/Beats/CreateCollector.md)|Creates a shipper.|
|[DescribeCollector](/intl.en-US/API Reference/Beats/DescribeCollector.md)|Queries the detailed information of a shipper.|
|[ReinstallCollector](/intl.en-US/API Reference/Beats/ReinstallCollector.md)|Installs a shipper that failed to be installed when you create the shipper.|
|[ListCollectors](/intl.en-US/API Reference/Beats/ListCollectors.md)|Queries shippers.|
|[ListDefaultCollectorConfigurations](/intl.en-US/API Reference/Beats/ListDefaultCollectorConfigurations.md)|Queries the default configuration files of shippers.|
|[UpdateCollectorName](/intl.en-US/API Reference/Beats/UpdateCollectorName.md)|Changes the name of a shipper.|
|[UpdateCollector](/intl.en-US/API Reference/Beats/UpdateCollector.md)|Updates the information of a shipper.|
|[StartCollector](/intl.en-US/API Reference/Beats/StartCollector.md)|Starts a shipper.|
|[RestartCollector](/intl.en-US/API Reference/Beats/RestartCollector.md)|Restarts a shipper.|
|[StopCollector](/intl.en-US/API Reference/Beats/StopCollector.md)|Stops a shipper.|
|[DeleteCollector](/intl.en-US/API Reference/Beats/DeleteCollector.md)|Deletes a shipper.|
|[ListEcsInstances](/intl.en-US/API Reference/Beats/ECS-based deployment/ListEcsInstances.md)|Queries Elastic Compute Service \(ECS\) instances.|
|[ModifyDeployMachine](/intl.en-US/API Reference/Beats/ECS-based deployment/ModifyDeployMachine.md)|Changes the ECS instance on which a shipper is installed.|
|[ListNodes](/intl.en-US/API Reference/Beats/ECS-based deployment/ListNodes.md)|Queries the statuses of ECS instances on which a shipper is installed.|
|[ListAckClusters](/intl.en-US/API Reference/Beats/ACK-based deployment/ListAckClusters.md)|Queries Container Service for Kubernetes \(ACK\) clusters.|
|[ListAckNamespaces](/intl.en-US/API Reference/Beats/ACK-based deployment/ListAckNamespaces.md)|Queries all the namespaces of a specified ACK cluster.|
|[DescribeAckOperator](/intl.en-US/API Reference/Beats/ACK-based deployment/DescribeAckOperator.md)|Queries the information of ES-operator installed for a specified ACK cluster.|
|[InstallAckOperator](/intl.en-US/API Reference/Beats/ACK-based deployment/InstallAckOperator.md)|Installs ES-operator for a specified ACK cluster.|

## Access control

|Operation|Description|
|---------|-----------|
|[InitializeOperationRole](/intl.en-US/API Reference/RAM/InitializeOperationRole.md)|Creates a service-linked role.|
|[ValidateSlrPermission](/intl.en-US/API Reference/RAM/ValidateSlrPermission.md)|Checks whether a service-linked role is created.|

