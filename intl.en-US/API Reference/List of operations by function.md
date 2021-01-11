# List of operations by function

This topic provides you with the Elasticsearch of Alibaba Cloud, Kibana. API operations. If you want to use an interface that is not described in this document, contact Alibaba Cloud Technical support engineers.

## Elasticsearch

|Category|API|Description|
|--------|---|-----------|
|Instance management|[createInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/createInstance.md)|Create an Elasticsearch instance.|
|[ListInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/ListInstance.md)|List all or specified instances in detail.|
|[DescribeInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/DescribeInstance.md)|Query detailed information about a specified Elasticsearch instance.|
|[RestartInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/RestartInstance.md)|Restart a specified Elasticsearch instance.|
|[UpdateInstance](/intl.en-US/API Reference/Elasticsearch instances/Upgrade or downgrade a cluster/UpdateInstance.md)|Modify the information of a specified database instance, such as the number of nodes, instance quota, instance name, and storage.|
|[UpdateInstanceChargeType](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/UpdateInstanceChargeType.md)|Change the billing method of a pay-as-you-go instance to subscription.|
|[UpdateDescription](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/UpdateDescription.md)|Update the name of a specified instance.|
|[DeleteInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/DeleteInstance.md)|Releases a specified pay-as-you-go Elasticsearch instance. After the instance is released, the physical resources of the instance is reclaimed. The data of the instance is deleted and cannot be recovered. The disks mounted to the instance nodes and the snapshots are released.|
|[RenewInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/RenewInstance.md)|Reneinstance to renew a subscription instance.|
|[ActivateZones](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/ActivateZones.md)|Restores the node in the disabled zone to ActivateZones. This parameter is only valid for multi-zone instances.|
|[DeactivateZones](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/DeactivateZones.md)|When multiple zones are available, you can bring some zones offline. And migrate the nodes from the original zone to another zone.|
|[InterruptElasticsearchTask](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/InterruptElasticsearchTask.md)|Interrupt a Elasticsearch instance. Valid only for instances in the active state. After an instance is interrupted, the instance enters the suspended state.|
|Tag management|[ListTagResources](/intl.en-US/API Reference/Elasticsearch instances/Manage cluster tags/ListTagResources.md)|Query tags that are attached to one or more resources.|
|[TagResources](/intl.en-US/API Reference/Elasticsearch instances/Manage cluster tags/TagResources.md)|Creates a relationship between tag resources.|
|[UntagResources](/intl.en-US/API Reference/Elasticsearch instances/Manage cluster tags/UntagResources.md)|Deletes the tag relationship of a user resource.|
|Data migration|[GetTransferableNodes](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/GetTransferableNodes.md)|Obtain the nodes that can be used for data migration, specifying the node type and the number of nodes.|
|[ValidateTransferableNodes](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/ValidateTransferableNodes.md)|Check whether data on certain nodes in a specified instance can be migrated.|
|[TransferNode](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/TransferNode.md)|Runs a data migration task.|
|[CancelTask](/intl.en-US/API Reference/Elasticsearch instances/Migrate Data/CancelTask.md)|Cancel a data migration task.|
|Instance upgrade and downgrade|[GetSuggestShrinkableNodes](/intl.en-US/API Reference/Elasticsearch instances/Upgrade or downgrade a cluster/GetSuggestShrinkableNodes.md)|Specify the node type and number to obtain the nodes that can be removed.|
|[ValidateShrinkNodes](/intl.en-US/API Reference/Elasticsearch instances/Upgrade or downgrade a cluster/ValidateShrinkNodes.md)|Check whether certain nodes in a specified instance can be scaled in.|
|[ShrinkNode](/intl.en-US/API Reference/Elasticsearch instances/Upgrade or downgrade a cluster/ShrinkNode.md)|Remove a node from a cluster.|
|[UpgradeEngineVersion](/intl.en-US/API Reference/Elasticsearch instances/Upgrade or downgrade a cluster/UpgradeEngineVersion.md)|Upgrade the Elasticsearch instance version or kernel patch version. You can only upgrade an instance from version 6.3 to Version 6.7.|
|Cluster Configuration|[UpdateInstanceSettings](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/UpdateInstanceSettings.md)|Update the YML configuration of a specified instance.|
|[UpdateHotIkDicts](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/UpdateHotIkDicts.md)|Update the IK dictionary for the Elasticsearch instance.|
|[UpdateSynonymsDicts](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/UpdateSynonymsDicts.md)|Update the Synonym Dictionary of the Elasticsearch instance.|
|[UpdateAdvancedSetting](/intl.en-US/API Reference/Elasticsearch instances/Configure clusters/UpdateAdvancedSetting.md)|Change the garbage collector configuration of a specified instance.|
|Plug-in management|[ListPlugins](/intl.en-US/API Reference/Elasticsearch instances/Manage plug-ins/ListPlugins.md)|Query plug-ins on a specified Elasticsearch instance.|
|[InstallSystemPlugin](/intl.en-US/API Reference/Elasticsearch instances/Manage plug-ins/InstallSystemPlugin.md)|Install a system preset plug-in.|
|[UninstallPlugin](/intl.en-US/API Reference/Elasticsearch instances/Manage plug-ins/UninstallPlugin.md)|Uninstall the preset plug-in.|
|Log query|[ListSearchLog](/intl.en-US/API Reference/Elasticsearch instances/Query logs/ListSearchLog.md)|View the instance log.|
|Security Settings|[TriggerNetwork](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/TriggerNetwork.md)|Enables or disables Internet or intranet access to Elasticsearch and Kibana.|
|[UpdatePrivateNetworkWhiteIps](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/UpdatePrivateNetworkWhiteIps.md)|Update the VPC whitelist for a specified instance.|
|[UpdatePublicWhiteIps](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/UpdatePublicWhiteIps.md)|Update the UpdatePublicWhiteIps whitelist of a specified instance.|
|[UpdatePublicNetwork](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/UpdatePublicNetwork.md)|Enable or disable the public endpoint of a specified instance.|
|[UpdateAdminPassword](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/UpdateAdminPassword.md)|Update the password of the elastic account of a specified instance.|
|[OpenHttps](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/OpenHttps.md)|Enable the HTTPS protocol. Before enabling a client node, make sure that you have purchased client nodes.|
|[CloseHttps](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/CloseHttps.md)|Close the HTTPS protocol.|
|[AddConnectableCluster](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/AddConnectableCluster.md)|Configure instance interconnection through AddConnectableCluster.|
|[DeleteConnectedCluster](/intl.en-US/API Reference/Elasticsearch instances/Configure security settings/DeleteConnectedCluster.md)|Remove the interconnected instance.|
|Data backup|[DescribeSnapshotSetting](/intl.en-US/API Reference/Elasticsearch instances/Backup data/DescribeSnapshotSetting.md)|Get the data backup configuration of the cluster.|
|[UpdateSnapshotSetting](/intl.en-US/API Reference/Elasticsearch instances/Backup data/UpdateSnapshotSetting.md)|Update the backup data of a specified instance.|
|[ListSnapshotReposByInstanceId](/intl.en-US/API Reference/Elasticsearch instances/Backup data/ListSnapshotReposByInstanceId.md)|Query the OSS repository configurations across clusters of the current instance.|
|[ListAlternativeSnapshotRepos](/intl.en-US/API Reference/Elasticsearch instances/Backup data/ListAlternativeSnapshotRepos.md)|Get the OSS reference warehouses that can be added to the current instance.|

## Kibana

|API|Description|
|---|-----------|
|[DescribeKibanaSettings](/intl.en-US/API Reference/Kibana/DescribeKibanaSettings.md)|Obtain the Kibana configuration.|
|[UpdateKibanaSettings](/intl.en-US/API Reference/Kibana/UpdateKibanaSettings.md)|Modify the Kibana configuration. Only the Kibana language can be modified.|
|[ListKibanaPlugins](/intl.en-US/API Reference/Kibana/ListKibanaPlugins.md)|Obtain the Kibana plug-in list.|
|[InstallKibanaSystemPlugin](/intl.en-US/API Reference/Kibana/InstallKibanaSystemPlugin.md)|Install the Kibana plug-in. The specification of Kibana must be 2-Core 4 GB or higher.|
|[UninstallKibanaPlugin](/intl.en-US/API Reference/Kibana/UninstallKibanaPlugin.md)|Uninstall the Kibana plug-in.|

