# API概览

本文为您提供阿里云Elasticsearch、Kibana、Logstash和Beats的API接口及说明。如果您需要使用本文中没有提到的接口 ，请联系阿里云技术支持工程师获取。

## Elasticsearch

|类别|API|描述|
|--|---|--|
|实例管理|[createInstance](/intl.zh-CN/API参考/Elasticsearch/实例管理/createInstance.md)|调用createInstance，创建Elasticsearch实例。|
|[ListInstance](/intl.zh-CN/API参考/Elasticsearch/实例管理/ListInstance.md)|调用ListInstance，在列表中展示所有或指定实例的详细信息。|
|[DescribeInstance](/intl.zh-CN/API参考/Elasticsearch/实例管理/DescribeInstance.md)|调用DescribeInstance，查询指定实例的详细信息。|
|[EstimatedRestartTime](/intl.zh-CN/API参考/Elasticsearch/实例管理/EstimatedRestartTime.md)|调用EstimatedRestartTime，获取重启实例的预估时间。|
|[RestartInstance](/intl.zh-CN/API参考/Elasticsearch/实例管理/RestartInstance.md)|调用RestartInstance，重启指定的阿里云Elasticsearch实例。|
|[UpdateInstanceChargeType](/intl.zh-CN/API参考/Elasticsearch/实例管理/UpdateInstanceChargeType.md)|调用UpdateInstanceChargeType，将按量付费实例转换为包年包月实例。|
|[UpdateDescription](/intl.zh-CN/API参考/Elasticsearch/实例管理/UpdateDescription.md)|调用UpdateDescription，更新指定实例的名称。|
|[DeleteInstance](/intl.zh-CN/API参考/Elasticsearch/实例管理/DeleteInstance.md)|调用DeleteInstance，释放指定按量付费类型的阿里云Elasticsearch实例。释放后，实例所使用的物理资源都被回收，相关数据全部丢失且不可恢复；挂载实例节点的云盘和相应的快照都会被释放。|
|[CancelDeletion](/intl.zh-CN/API参考/Elasticsearch/实例管理/CancelDeletion.md)|调用CancelDeletion，恢复释放后被冻结的Elasticsearch实例。|
|[RenewInstance](/intl.zh-CN/API参考/Elasticsearch/实例管理/RenewInstance.md)|调用RenewInstance，为包年包月实例续费。|
|[ActivateZones](/intl.zh-CN/API参考/Elasticsearch/实例管理/ActivateZones.md)|调用ActivateZones，恢复已下线的可用区中的节点。仅对多可用区实例有效。|
|[DeactivateZones](/intl.zh-CN/API参考/Elasticsearch/实例管理/DeactivateZones.md)|调用DeactivateZones，在有多个可用区的情况下，下线部分可用区。并将下线的可用区中的节点迁移到其他可用区。|
|[DescribeRegions](/intl.zh-CN/API参考/Elasticsearch/实例管理/DescribeRegions.md)|调用DescribeRegions，获取阿里云Elasticsearch的区域信息。|
|[InterruptElasticsearchTask](/intl.zh-CN/API参考/Elasticsearch/实例管理/InterruptElasticsearchTask.md)|调用InterruptElasticsearchTask，中断变更中的阿里云Elasticsearch实例。仅对状态为生效中的实例有效，中断后，实例进入变更中断（suspended）状态。|
|[ResumeElasticsearchTask](/intl.zh-CN/API参考/Elasticsearch/实例管理/ResumeElasticsearchTask.md)|调用ResumeElasticsearchTask，恢复中断变更的阿里云Elasticsearch实例。|
|[ListAllNode](/intl.zh-CN/API参考/Elasticsearch/实例管理/ListAllNode.md)|调用ListAllNode，获取集群下的所有节点信息。|
|[DescribeElasticsearchHealth](/intl.zh-CN/API参考/Elasticsearch/实例管理/DescribeElasticsearchHealth.md)|调用DescribeElasticsearchHealth，获取指定Elasticsearch实例的健康情况。|
|[ListInstanceIndices](/intl.zh-CN/API参考/Elasticsearch/实例管理/ListInstanceIndices.md)|调用ListInstanceIndices，获取集群的索引列表。|
|[MigrateToOtherZone](/intl.zh-CN/API参考/Elasticsearch/实例管理/MigrateToOtherZone.md)|调用MigrateToOtherZone，迁移对应可用区下的节点到目标可用区。|
|[MoveResourceGroup](/intl.zh-CN/API参考/Elasticsearch/实例管理/MoveResourceGroup.md)|调用MoveResourceGroup，迁移实例到指定资源组。|
|[ModifyInstanceMaintainTime](/intl.zh-CN/API参考/Elasticsearch/实例管理/ModifyInstanceMaintainTime.md)|调用ModifyInstanceMaintainTime，更改并开启实例的可维护时间。|
|[GetRegionConfiguration](/intl.zh-CN/API参考/Elasticsearch/实例管理/GetRegionConfiguration.md)|获取当前区域的开放配置信息。接口返回值为全量数据供参考，以控制台和售卖页实际展示值为准。|
|标签管理|[ListTags](/intl.zh-CN/API参考/Elasticsearch/标签管理/ListTags.md)|调用ListTags，查询所有可见的用户标签。|
|[ListTagResources](/intl.zh-CN/API参考/Elasticsearch/标签管理/ListTagResources.md)|调用ListTagResources，查询可见的资源标签关系。|
|[TagResources](/intl.zh-CN/API参考/Elasticsearch/标签管理/TagResources.md)|调用TagResources，创建标签资源关系。|
|[UntagResources](/intl.zh-CN/API参考/Elasticsearch/标签管理/UntagResources.md)|调用UntagResources，删除用户资源标签关系。|
|数据迁移|[GetTransferableNodes](/intl.zh-CN/API参考/Elasticsearch/数据迁移/GetTransferableNodes.md)|调用GetTransferableNodes，指定节点类型和个数，获取可进行数据迁移的节点。|
|[ValidateTransferableNodes](/intl.zh-CN/API参考/Elasticsearch/数据迁移/ValidateTransferableNodes.md)|调用ValidateTransferableNodes，校验是否可以迁移指定实例中某些节点上的数据。|
|[TransferNode](/intl.zh-CN/API参考/Elasticsearch/数据迁移/TransferNode.md)|调用TransferNode，执行数据迁移任务。|
|[ListDataTasks](/intl.zh-CN/API参考/Elasticsearch/数据迁移/ListDataTasks.md)|调用ListDataTasks，获取数据迁移任务信息。|
|[CreateDataTasks](/intl.zh-CN/API参考/Elasticsearch/数据迁移/CreateDataTasks.md)|调用CreateDataTasks，创建索引迁移任务，将所选集群中的数据迁移到当前集群。|
|[GetClusterDataInformation](/intl.zh-CN/API参考/Elasticsearch/数据迁移/GetClusterDataInformation.md)|调用GetClusterDataInformation，获取集群的数据信息。|
|[DeleteDataTask](/intl.zh-CN/API参考/Elasticsearch/数据迁移/DeleteDataTask.md)|调用DeleteDataTask，删除索引迁移任务。|
|[CancelTask](/intl.zh-CN/API参考/Elasticsearch/数据迁移/CancelTask.md)|调用CancelTask，取消数据迁移任务。|
|实例升降配|[GetSuggestShrinkableNodes](/intl.zh-CN/API参考/Elasticsearch/实例升降配/GetSuggestShrinkableNodes.md)|调用GetSuggestShrinkableNodes，指定节点类型和数量，获取可缩容的节点。|
|[ValidateShrinkNodes](/intl.zh-CN/API参考/Elasticsearch/实例升降配/ValidateShrinkNodes.md)|调用ValidateShrinkNodes，校验指定实例中的某些节点是否可以缩容。|
|[ShrinkNode](/intl.zh-CN/API参考/Elasticsearch/实例升降配/ShrinkNode.md)|调用ShrinkNode，执行集群节点缩容操作。|
|[UpgradeEngineVersion](/intl.zh-CN/API参考/Elasticsearch/实例升降配/UpgradeEngineVersion.md)|调用UpgradeEngineVersion，升级Elasticsearch的实例版本或内核补丁版本。升级实例版本功能仅支持将6.3版本的实例升级至6.7版本。|
|[UpdateInstance](/intl.zh-CN/API参考/Elasticsearch/实例升降配/UpdateInstance.md)|调用UpdateInstance，变更集群配置（升配或降配）。|
|集群配置|[UpdateInstanceSettings](/intl.zh-CN/API参考/Elasticsearch/集群配置/UpdateInstanceSettings.md)|调用UpdateInstanceSettings，更新指定实例的YML参数配置。|
|[UpdateHotIkDicts](/intl.zh-CN/API参考/Elasticsearch/集群配置/UpdateHotIkDicts.md)|调用UpdateHotIkDicts，更新阿里云Elasticsearch实例的IK热词词典。|
|[UpdateSynonymsDicts](/intl.zh-CN/API参考/Elasticsearch/集群配置/UpdateSynonymsDicts.md)|调用UpdateSynonymsDicts，更新阿里云Elasticsearch实例的同义词词典。|
|[UpdateDict](/intl.zh-CN/API参考/Elasticsearch/集群配置/UpdateDict.md)|调用UpdateDict，更新Elasticsearch实例的用户词典。|
|[UpdateAliwsDict](/intl.zh-CN/API参考/Elasticsearch/集群配置/UpdateAliwsDict.md)|调用UpdateAliwsDict，更新AliNLP分词插件（analysis-aliws）的词典文件。支持自定义词库配置。|
|[ListDictInformation](/intl.zh-CN/API参考/Elasticsearch/集群配置/ListDictInformation.md)|调用ListDictInformation，在添加用户OSS存储的词典文件时，获取和校验用户OSS词典文件的详情。|
|[UpdateAdvancedSetting](/intl.zh-CN/API参考/Elasticsearch/集群配置/UpdateAdvancedSetting.md)|调用UpdateAdvancedSetting，更改指定实例的垃圾回收器配置。|
|插件管理|[ListPlugins](/intl.zh-CN/API参考/Elasticsearch/插件管理/ListPlugins.md)|调用ListPlugins，获取指定阿里云Elasticsearch实例的插件列表。|
|[InstallSystemPlugin](/intl.zh-CN/API参考/Elasticsearch/插件管理/InstallSystemPlugin.md)|调用InstallSystemPlugin，安装系统预置插件。|
|[UninstallPlugin](/intl.zh-CN/API参考/Elasticsearch/插件管理/UninstallPlugin.md)|调用UninstallPlugin，卸载已安装的预置插件。|
|[InstallUserPlugins](/intl.zh-CN/API参考/Elasticsearch/插件管理/InstallUserPlugins.md)|调用InstallUserPlugins，安装用户自定义的已经上传至Elasticsearch控制台的插件。|
|日志查询|[ListSearchLog](/intl.zh-CN/API参考/Elasticsearch/日志查询/ListSearchLog.md)|调用ListSearchLog，查看实例日志。|
|安全配置|[TriggerNetwork](/intl.zh-CN/API参考/Elasticsearch/安全配置/TriggerNetwork.md)|调用TriggerNetwork，开启或关闭Elasticsearch、Kibana的公网或私网访问。|
|[UpdatePrivateNetworkWhiteIps](/intl.zh-CN/API参考/Elasticsearch/安全配置/UpdatePrivateNetworkWhiteIps.md)|调用UpdatePrivateNetworkWhiteIps，更新指定实例的VPC私网访问白名单。|
|[UpdatePublicWhiteIps](/intl.zh-CN/API参考/Elasticsearch/安全配置/UpdatePublicWhiteIps.md)|调用UpdatePublicWhiteIps，更新指定实例的公网地址访问白名单。|
|[UpdatePublicNetwork](/intl.zh-CN/API参考/Elasticsearch/安全配置/UpdatePublicNetwork.md)|调用UpdatePublicNetwork，开启或关闭指定实例的公网地址。|
|[UpdateWhiteIps](/intl.zh-CN/API参考/Elasticsearch/安全配置/UpdateWhiteIps.md)|调用UpdateWhiteIps，更新Elasticsearch实例的VPC私网访问白名单。|
|[ModifyWhiteIps](/intl.zh-CN/API参考/Elasticsearch/安全配置/ModifyWhiteIps.md)|调用ModifyWhiteIps，更新指定实例的访问白名单。|
|[UpdateAdminPassword](/intl.zh-CN/API参考/Elasticsearch/安全配置/UpdateAdminPassword.md)|调用UpdateAdminPassword，更新指定实例的elastic账号的密码。|
|[OpenHttps](/intl.zh-CN/API参考/Elasticsearch/安全配置/OpenHttps.md)|调用OpenHttps，开启HTTPS协议。开启前请确保您已购买协调节点。|
|[CloseHttps](/intl.zh-CN/API参考/Elasticsearch/安全配置/CloseHttps.md)|调用CloseHttps，关闭HTTPS协议。|
|[AddConnectableCluster](/intl.zh-CN/API参考/Elasticsearch/安全配置/AddConnectableCluster.md)|调用AddConnectableCluster，配置实例网络互通。|
|[DeleteConnectedCluster](/intl.zh-CN/API参考/Elasticsearch/安全配置/DeleteConnectedCluster.md)|调用DeleteConnectedCluster，移除互通实例。|
|[DescribeConnectableClusters](/intl.zh-CN/API参考/Elasticsearch/安全配置/DescribeConnectableClusters.md)|调用DescribeConnectableClusters，获取能够与当前实例进行网络互通的实例列表。不包括已经打通的实例。|
|[ListConnectedClusters](/intl.zh-CN/API参考/Elasticsearch/安全配置/ListConnectedClusters.md)|调用ListConnectedClusters，获取已经与当前实例进行了网络互通的实例列表。|
|数据备份|[CreateSnapshot](/intl.zh-CN/API参考/Elasticsearch/数据备份/CreateSnapshot.md)|调用CreateSnapshot，手动对集群进行快照备份。|
|[DescribeSnapshotSetting](/intl.zh-CN/API参考/Elasticsearch/数据备份/DescribeSnapshotSetting.md)|调用DescribeSnapshotSetting，获取集群的数据备份配置。|
|[UpdateSnapshotSetting](/intl.zh-CN/API参考/Elasticsearch/数据备份/UpdateSnapshotSetting.md)|调用UpdateSnapshotSetting，更新指定实例的数据备份配置。|
|[ListSnapshotReposByInstanceId](/intl.zh-CN/API参考/Elasticsearch/数据备份/ListSnapshotReposByInstanceId.md)|调用ListSnapshotReposByInstanceId，获取当前实例的跨集群OSS仓库设置列表。|
|[ListAlternativeSnapshotRepos](/intl.zh-CN/API参考/Elasticsearch/数据备份/ListAlternativeSnapshotRepos.md)|调用ListAlternativeSnapshotRepos，获取当前实例可添加的OSS引用仓库。|
|[AddSnapshotRepo](/intl.zh-CN/API参考/Elasticsearch/数据备份/AddSnapshotRepo.md)|调用AddSnapshotRepo，在设置跨集群OSS仓库时，创建引用仓库。|
|[DeleteSnapshotRepo](/intl.zh-CN/API参考/Elasticsearch/数据备份/DeleteSnapshotRepo.md)|调用DeleteSnapRepo，删除一个跨集群OSS引用仓库。|
|智能运维|[OpenDiagnosis](/intl.zh-CN/API参考/Elasticsearch/智能运维/OpenDiagnosis.md)|调用OpenDiagnosis，打开实例的智能运维功能。|
|[CloseDiagnosis](/intl.zh-CN/API参考/Elasticsearch/智能运维/CloseDiagnosis.md)|调用CloseDiagnosis，关闭实例的智能运维功能。|
|[DiagnoseInstance](/intl.zh-CN/API参考/Elasticsearch/智能运维/DiagnoseInstance.md)|调用DiagnoseInstance，即刻诊断实例。|
|[ListDiagnoseReport](/intl.zh-CN/API参考/Elasticsearch/智能运维/ListDiagnoseReport.md)|调用ListDiagnoseReport，获取智能运维的历史报告。|
|[ListDiagnoseReportIds](/intl.zh-CN/API参考/Elasticsearch/智能运维/ListDiagnoseReportIds.md)|调用ListDiagnoseReportIds，获取智能运维历史报告的ID。|
|[ListDiagnoseIndices](/intl.zh-CN/API参考/Elasticsearch/智能运维/ListDiagnoseIndices.md)|调用ListDiagnoseIndices，获取指定实例智能运维模块中，健康诊断的诊断索引。|
|[DescribeDiagnoseReport](/intl.zh-CN/API参考/Elasticsearch/智能运维/DescribeDiagnoseReport.md)|调用DescribeDiagnoseReport，查看智能运维的历史报告。|
|[DescribeDiagnosisSettings](/intl.zh-CN/API参考/Elasticsearch/智能运维/DescribeDiagnosisSettings.md)|调用DescribeDiagnosisSettings，获取智能运维的场景设置。|
|[UpdateDiagnosisSettings](/intl.zh-CN/API参考/Elasticsearch/智能运维/UpdateDiagnosisSettings.md)|调用UpdateDiagnosisSettings，更新实例的智能运维场景设置。|

## Kibana

|API|描述|
|---|--|
|[DescribeKibanaSettings](/intl.zh-CN/API参考/Kibana/DescribeKibanaSettings.md)|调用DescribeKibanaSettings，获取Kibana配置。|
|[UpdateKibanaSettings](/intl.zh-CN/API参考/Kibana/UpdateKibanaSettings.md)|调用UpdateKibanaSettings，修改Kibana配置。目前仅支持修改Kibana语言配置。|
|[ListKibanaPlugins](/intl.zh-CN/API参考/Kibana/ListKibanaPlugins.md)|调用ListKibanaPlugins，获取Kibana插件列表。|
|[InstallKibanaSystemPlugin](/intl.zh-CN/API参考/Kibana/InstallKibanaSystemPlugin.md)|调用InstallKibanaSystemPlugin，安装Kibana预置插件。要求Kibana的规格为2核4G及以上。|
|[UninstallKibanaPlugin](/intl.zh-CN/API参考/Kibana/UninstallKibanaPlugin.md)|调用UninstallKibanaPlugin，卸载Kibana插件。|
|[UpdateKibanaWhiteIps](/intl.zh-CN/API参考/Kibana/UpdateKibanaWhiteIps.md)|调用UpdateKibanaWhiteIps，更新指定阿里云Elasticsearch实例的Kibana访问白名单。|

## Logstash

|类别|API|描述|
|--|---|--|
|实例管理|[CreateLogstash](/intl.zh-CN/API参考/Logstash/实例管理/CreateLogstash.md)|调用CreateLogstash，创建一个Logstash实例。|
|[ListLogstash](/intl.zh-CN/API参考/Logstash/实例管理/ListLogstash.md)|调用ListLogstash，在列表中展示所有或指定Logstash实例的详细信息。|
|[DescribeLogstash](/intl.zh-CN/API参考/Logstash/实例管理/DescribeLogstash.md)|调用DescribeLogstash，查询指定实例的详细信息。|
|[UpdateLogstash](/intl.zh-CN/API参考/Logstash/实例管理/UpdateLogstash.md)|调用UpdateLogstash，修改指定实例的部分信息，例如节点数、配额、名称、硬盘大小等。|
|[RenewLogstash](/intl.zh-CN/API参考/Logstash/实例管理/RenewLogstash.md)|调用RenewLogstash，为实例续费。|
|[RestartLogstash](/intl.zh-CN/API参考/Logstash/实例管理/RestartLogstash.md)|调用RestartLogstash，重启指定实例。重启后，实例会进入生效中（activating）状态。|
|[UpdateLogstashDescription](/intl.zh-CN/API参考/Logstash/实例管理/UpdateLogstashDescription.md)|调用UpdateLogstashDescription，修改指定Logstash实例的名称。|
|[UpdateLogstashChargeType](/intl.zh-CN/API参考/Logstash/实例管理/UpdateLogstashChargeType.md)|调用UpdateLogstashChargeType，将按量付费的阿里云Logstash实例转换为包年包月实例。|
|[EstimatedLogstashRestartTime](/intl.zh-CN/API参考/Logstash/实例管理/EstimatedLogstashRestartTime.md)|调用EstimatedLogstashRestartTime，获取Logstash实例重启的预估时间。|
|[DeleteLogstash](/intl.zh-CN/API参考/Logstash/实例管理/DeleteLogstash.md)|调用DeleteLogstash，释放指定实例。|
|[CancelLogstashDeletion](/intl.zh-CN/API参考/Logstash/实例管理/CancelLogstashDeletion.md)|调用CancelLogstashDeletion，恢复释放后被冻结的Logstash实例。|
|集群配置|[UpdateLogstashSettings](/intl.zh-CN/API参考/Logstash/集群配置/UpdateLogstashSettings.md)|调用UpdateLogstashSettings，更新指定Logstash实例的配置。|
|[ListExtendfiles](/intl.zh-CN/API参考/Logstash/集群配置/ListExtendfiles.md)|调用ListExtendfiles，获取Logstash实例的扩展文件配置。|
|[UpdateExtendfiles](/intl.zh-CN/API参考/Logstash/集群配置/UpdateExtendfiles.md)|调用UpdateExtendfiles，更新Logstash实例的扩展文件配置。|
|集群监控|[ListAvailableEsInstanceIds](/intl.zh-CN/API参考/Logstash/集群监控/ListAvailableEsInstanceIds.md)|调用ListAvailableEsInstanceIds，在设置Logstash实例的X-Pack监控时，获取可用的Elasticsearch实例列表（具备X-Pack监控能力）。|
|[ValidateConnection](/intl.zh-CN/API参考/Logstash/集群监控/ValidateConnection.md)|调用ValidateConnection，在Logstash实例的监控报警配置中，验证提供X-Pack监控的Elasticsearch实例的联通性。|
|[UpdateXpackMonitorConfig](/intl.zh-CN/API参考/Logstash/集群监控/UpdateXpackMonitorConfig.md)|调用UpdateXpackMonitorConfig，更新Logstash实例的X-Pack监控报警配置。|
|[DescribeXpackMonitorConfig](/intl.zh-CN/API参考/Logstash/集群监控/DescribeXpackMonitorConfig.md)|调用DescribeXpackMonitorConfig，获取Logstash实例的X-Pack监控配置。|
|插件管理|[ListLogstashPlugins](/intl.zh-CN/API参考/Logstash/插件管理/ListLogstashPlugins.md)|调用ListLogstashPlugins，获取所有或指定插件的详细信息。|
|[InstallLogstashSystemPlugin](/intl.zh-CN/API参考/Logstash/插件管理/InstallLogstashSystemPlugin.md)|调用InstallLogstashSystemPlugin，安装插件。|
|[UninstallLogstashPlugin](/intl.zh-CN/API参考/Logstash/插件管理/UninstallLogstashPlugin.md)|调用UninstallLogstashPlugin，卸载已安装的插件。|
|日志查询|[ListLogstashLog](/intl.zh-CN/API参考/Logstash/日志查询/ListLogstashLog.md)|调用ListLogstashLog，查看Logstash实例的日志。|
|变更任务管理|[InterruptLogstashTask](/intl.zh-CN/API参考/Logstash/变更任务管理/InterruptLogstashTask.md)|调用InterruptLogstashTask，中断实例变更任务。中断后，实例会进入中断中（suspended）状态。|
|[ResumeLogstashTask](/intl.zh-CN/API参考/Logstash/变更任务管理/ResumeLogstashTask.md)|调用ResumeLogstashTask，恢复实例的变更中断任务。恢复后实例会进入生效中（activating）状态。|
|管道管理|[CreatePipelines](/intl.zh-CN/API参考/Logstash/管道管理/CreatePipelines.md)|调用CreatePipelines，创建Logstash管道。|
|[ListPipeline](/intl.zh-CN/API参考/Logstash/管道管理/ListPipeline.md)|调用ListPipeline，获取Logstash实例的管道列表。|
|[DescribePipeline](/intl.zh-CN/API参考/Logstash/管道管理/DescribePipeline.md)|调用DescribePipeline，获取Logstash实例的管道信息。|
|[UpdatePipelines](/intl.zh-CN/API参考/Logstash/管道管理/UpdatePipelines.md)|调用UpdatePipelines，更新Logstash管道信息。|
|[RunPipelines](/intl.zh-CN/API参考/Logstash/管道管理/RunPipelines.md)|调用RunPipelines，立即部署Logstash管道。|
|[StopPipelines](/intl.zh-CN/API参考/Logstash/管道管理/StopPipelines.md)|调用StopPipelines，停止运行Logstash管道。|
|[UpdatePipelineManagementConfig](/intl.zh-CN/API参考/Logstash/管道管理/UpdatePipelineManagementConfig.md)|调用UpdatePipelineManagementConfig，更新Logstash管道管理方式。|
|[DescribePipelineManagementConfig](/intl.zh-CN/API参考/Logstash/管道管理/DescribePipelineManagementConfig.md)|调用DescribePipelineManagementConfig，获取Logstash管道管理配置。|
|[DeletePipelines](/intl.zh-CN/API参考/Logstash/管道管理/DeletePipelines.md)|调用DeletePipelines，删除指定的Logstash管道。|

## Beats

|API|描述|
|---|--|
|[CreateCollector](/intl.zh-CN/API参考/Beats/CreateCollector.md)|调用CreateCollector，创建采集器。|
|[DescribeCollector](/intl.zh-CN/API参考/Beats/DescribeCollector.md)|调用DescribeCollector，获取采集器实例的详细信息。|
|[ReinstallCollector](/intl.zh-CN/API参考/Beats/ReinstallCollector.md)|调用ReinstallCollector，重试安装在创建时没有安装成功的采集器。|
|[ListCollectors](/intl.zh-CN/API参考/Beats/ListCollectors.md)|调用ListCollectors，获取采集器列表信息。|
|[ListDefaultCollectorConfigurations](/intl.zh-CN/API参考/Beats/ListDefaultCollectorConfigurations.md)|调用ListDefaultCollectorConfigurations，获取采集器的默认配置文件。|
|[UpdateCollectorName](/intl.zh-CN/API参考/Beats/UpdateCollectorName.md)|调用UpdateCollectorName，修改采集器名称。|
|[UpdateCollector](/intl.zh-CN/API参考/Beats/UpdateCollector.md)|调用UpdateCollector，更新采集器实例信息。|
|[StartCollector](/intl.zh-CN/API参考/Beats/StartCollector.md)|调用StartCollector，启动采集器。|
|[RestartCollector](/intl.zh-CN/API参考/Beats/RestartCollector.md)|调用RestartCollector，重启采集器。|
|[StopCollector](/intl.zh-CN/API参考/Beats/StopCollector.md)|调用StopCollector，停止运行中的采集器。|
|[DeleteCollector](/intl.zh-CN/API参考/Beats/DeleteCollector.md)|调用DeleteCollector，删除采集器。|
|[ListEcsInstances](/intl.zh-CN/API参考/Beats/基于ECS部署/ListEcsInstances.md)|调用ListEcsInstances，获取ECS机器列表。|
|[ModifyDeployMachine](/intl.zh-CN/API参考/Beats/基于ECS部署/ModifyDeployMachine.md)|调用ModifyDeployMachine，更新采集器安装的ECS机器。|
|[ListNodes](/intl.zh-CN/API参考/Beats/基于ECS部署/ListNodes.md)|调用ListNodes，查看安装采集器的ECS机器的状态。|
|[ListAckClusters](/intl.zh-CN/API参考/Beats/基于ACK部署/ListAckClusters.md)|调用ListAckClusters，获取容器服务Kubernetes版ACK（Container Service for Kubernetes）集群列表。|
|[ListAckNamespaces](/intl.zh-CN/API参考/Beats/基于ACK部署/ListAckNamespaces.md)|调用ListAckNamespaces，查看指定容器服务Kubernetes版ACK集群的所有命名空间。|
|[DescribeAckOperator](/intl.zh-CN/API参考/Beats/基于ACK部署/DescribeAckOperator.md)|调用DescribeAckOperator，查看指定容器服务Kubernetes版ACK集群上安装的Elasticsearch Operator信息。|
|[InstallAckOperator](/intl.zh-CN/API参考/Beats/基于ACK部署/InstallAckOperator.md)|调用InstallAckOperator，在指定容器服务Kubernetes版ACK集群上安装Elasticsearch Operator。|

## 访问控制

|API|描述|
|---|--|
|[InitializeOperationRole](/intl.zh-CN/API参考/访问控制/InitializeOperationRole.md)|调用InitializeOperationRole，创建服务关联角色。|
|[ValidateSlrPermission](/intl.zh-CN/API参考/访问控制/ValidateSlrPermission.md)|调用ValidateSlrPermission，验证是否已经创建服务关联角色。|

