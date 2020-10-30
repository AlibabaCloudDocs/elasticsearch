# API概览

本文为您提供阿里云Elasticsearch、Kibana、Logstash和Beats的API接口及说明。如果您需要使用本文中没有提到的接口 ，请联系阿里云技术支持工程师获取。

## Elasticsearch

|类别|API|描述|
|--|---|--|
|实例管理|[createInstance](/cn.zh-CN/API参考/Elasticsearch/实例管理/createInstance.md)|调用createInstance，创建Elasticsearch实例。|
|[ListInstance](/cn.zh-CN/API参考/Elasticsearch/实例管理/ListInstance.md)|调用ListInstance，在列表中展示所有或指定实例的详细信息。|
|[DescribeInstance](/cn.zh-CN/API参考/Elasticsearch/实例管理/DescribeInstance.md)|调用DescribeInstance，查询指定实例的详细信息。|
|[EstimatedRestartTime](/cn.zh-CN/API参考/Elasticsearch/实例管理/EstimatedRestartTime.md)|调用EstimatedRestartTime，获取重启实例的预估时间。|
|[RestartInstance](/cn.zh-CN/API参考/Elasticsearch/实例管理/RestartInstance.md)|调用RestartInstance，重启指定的阿里云Elasticsearch实例。|
|[UpdateInstanceChargeType](/cn.zh-CN/API参考/Elasticsearch/实例管理/UpdateInstanceChargeType.md)|调用UpdateInstanceChargeType，将按量付费实例转换为包年包月实例。|
|[UpdateDescription](/cn.zh-CN/API参考/Elasticsearch/实例管理/UpdateDescription.md)|调用UpdateDescription，更新指定实例的名称。|
|[DeleteInstance](/cn.zh-CN/API参考/Elasticsearch/实例管理/DeleteInstance.md)|调用DeleteInstance，释放指定按量付费类型的阿里云Elasticsearch实例。释放后，实例所使用的物理资源都被回收，相关数据全部丢失且不可恢复；挂载实例节点的云盘和相应的快照都会被释放。|
|[RenewInstance](/cn.zh-CN/API参考/Elasticsearch/实例管理/RenewInstance.md)|调用RenewInstance，为包年包月实例续费。|
|[ActivateZones](/cn.zh-CN/API参考/Elasticsearch/实例管理/ActivateZones.md)|调用ActivateZones，恢复已下线的可用区中的节点。仅对多可用区实例有效。|
|[DeactivateZones](/cn.zh-CN/API参考/Elasticsearch/实例管理/DeactivateZones.md)|调用DeactivateZones，在有多个可用区的情况下，下线部分可用区。并将下线的可用区中的节点迁移到其他可用区。|
|[DescribeRegions](/cn.zh-CN/API参考/Elasticsearch/实例管理/DescribeRegions.md)|调用DescribeRegions，获取阿里云Elasticsearch的区域信息。|
|[UpdateReadWritePolicy](/cn.zh-CN/API参考/Elasticsearch/实例管理/UpdateReadWritePolicy.md)|调用UpdateReadWritePolicy，开启或关闭集群的写入高可用特性。目前仅支持华北2（北京）区域的实例。|
|[InterruptElasticsearchTask](/cn.zh-CN/API参考/Elasticsearch/实例管理/InterruptElasticsearchTask.md)|调用InterruptElasticsearchTask，中断变更中的阿里云Elasticsearch实例。仅对状态为生效中的实例有效，中断后，实例进入变更中断（suspended）状态。|
|[ResumeElasticsearchTask](/cn.zh-CN/API参考/Elasticsearch/实例管理/ResumeElasticsearchTask.md)|调用ResumeElasticsearchTask，恢复中断变更的阿里云Elasticsearch实例。|
|[ListAllNode](/cn.zh-CN/API参考/Elasticsearch/实例管理/ListAllNode.md)|调用ListAllNode，获取集群下的所有节点信息。|
|[GetElastictask](/cn.zh-CN/API参考/Elasticsearch/实例管理/GetElastictask.md)|调用GetElastictask，获取集群的弹性扩缩容规则。必须在创建实例时购买弹性节点，才可调用此接口。|
|[ModifyElastictask](/cn.zh-CN/API参考/Elasticsearch/实例管理/ModifyElastictask.md)|调用ModifyElastictask，更新集群弹性扩缩容规则。|
|[ListInstanceIndices](/cn.zh-CN/API参考/Elasticsearch/实例管理/ListInstanceIndices.md)|调用ListInstanceIndices，获取集群的索引列表。|
|[MigrateToOtherZone](/cn.zh-CN/API参考/Elasticsearch/实例管理/MigrateToOtherZone.md)|调用MigrateToOtherZone，迁移对应可用区下的节点到目标可用区。|
|[ModifyInstanceMaintainTime](/cn.zh-CN/API参考/Elasticsearch/实例管理/ModifyInstanceMaintainTime.md)|调用ModifyInstanceMaintainTime，更改并开启实例的可维护时间。|
|标签管理|[ListTags](/cn.zh-CN/API参考/Elasticsearch/标签管理/ListTags.md)|调用ListTags，查询所有可见的用户标签。|
|[ListTagResources](/cn.zh-CN/API参考/Elasticsearch/标签管理/ListTagResources.md)|调用ListTagResources，查询可见的资源标签关系。|
|[TagResources](/cn.zh-CN/API参考/Elasticsearch/标签管理/TagResources.md)|调用TagResources，创建标签资源关系。|
|[UntagResources](/cn.zh-CN/API参考/Elasticsearch/标签管理/UntagResources.md)|调用UntagResources，删除用户资源标签关系。|
|数据迁移|[GetTransferableNodes](/cn.zh-CN/API参考/Elasticsearch/数据迁移/GetTransferableNodes.md)|调用GetTransferableNodes，指定节点类型和个数，获取可进行数据迁移的节点。|
|[ValidateTransferableNodes](/cn.zh-CN/API参考/Elasticsearch/数据迁移/ValidateTransferableNodes.md)|调用ValidateTransferableNodes，校验是否可以迁移指定实例中某些节点上的数据。|
|[TransferNode](/cn.zh-CN/API参考/Elasticsearch/数据迁移/TransferNode.md)|调用TransferNode，执行数据迁移任务。|
|[ListDataTasks]()|调用ListDataTasks，获取数据迁移任务信息。|
|[t1950899.md\#]()|调用CreateDataTasks，创建索引迁移任务，将所选集群中的数据迁移到当前集群。|
|[GetClusterDataInformation]()|调用GetClusterDataInformation，获取集群的数据信息。|
|[DeleteDataTask]()|调用DeleteDataTask，删除索引迁移任务。|
|[CancelTask](/cn.zh-CN/API参考/Elasticsearch/数据迁移/CancelTask.md)|调用CancelTask，取消数据迁移任务。|
|实例升降配|[GetSuggestShrinkableNodes](/cn.zh-CN/API参考/Elasticsearch/实例升降配/GetSuggestShrinkableNodes.md)|调用GetSuggestShrinkableNodes，指定节点类型和数量，获取可缩容的节点。|
|[ValidateShrinkNodes](/cn.zh-CN/API参考/Elasticsearch/实例升降配/ValidateShrinkNodes.md)|调用ValidateShrinkNodes，校验指定实例中的某些节点是否可以缩容。|
|[ShrinkNode](/cn.zh-CN/API参考/Elasticsearch/实例升降配/ShrinkNode.md)|调用ShrinkNode，执行集群节点缩容操作。|
|[UpgradeEngineVersion](/cn.zh-CN/API参考/Elasticsearch/实例升降配/UpgradeEngineVersion.md)|调用UpgradeEngineVersion，升级Elasticsearch的实例版本或内核补丁版本。升级实例版本功能仅支持将6.3版本的实例升级至6.7版本。|
|[UpdateInstance](/cn.zh-CN/API参考/Elasticsearch/实例升降配/UpdateInstance.md)|调用UpdateInstance，升配集群。|
|集群配置|[UpdateInstanceSettings](/cn.zh-CN/API参考/Elasticsearch/集群配置/UpdateInstanceSettings.md)|调用UpdateInstanceSettings，更新指定实例的YML参数配置。|
|[UpdateHotIkDicts](/cn.zh-CN/API参考/Elasticsearch/集群配置/UpdateHotIkDicts.md)|调用UpdateHotIkDicts，更新阿里云Elasticsearch实例的IK热词词典。|
|[UpdateSynonymsDicts](/cn.zh-CN/API参考/Elasticsearch/集群配置/UpdateSynonymsDicts.md)|调用UpdateSynonymsDicts，更新阿里云Elasticsearch实例的同义词词典。|
|[UpdateDict](/cn.zh-CN/API参考/Elasticsearch/集群配置/UpdateDict.md)|调用UpdateDict，更新Elasticsearch实例的用户词典。|
|[UpdateAdvancedSetting](/cn.zh-CN/API参考/Elasticsearch/集群配置/UpdateAdvancedSetting.md)|调用UpdateAdvancedSetting，更改指定实例的垃圾回收器配置。|
|[DescribeTemplates]()|调用DescribeTemplates，获取实例的场景模板配置。|
|[UpdateExtendConfig](/cn.zh-CN/API参考/Elasticsearch/集群配置/UpdateExtendConfig.md)|调用UpdateExtendConfig，修改集群的场景化配置模板。|
|[UpdateTemplate](/cn.zh-CN/API参考/Elasticsearch/集群配置/UpdateTemplate.md)|调用UpdateTemplate，修改集群的场景化模板配置内容。|
|插件管理|[ListPlugins](/cn.zh-CN/API参考/Elasticsearch/插件管理/ListPlugins.md)|调用ListPlugins，获取指定阿里云Elasticsearch实例的插件列表。|
|[InstallSystemPlugin](/cn.zh-CN/API参考/Elasticsearch/插件管理/InstallSystemPlugin.md)|调用InstallSystemPlugin，安装系统预置插件。|
|[UninstallPlugin](/cn.zh-CN/API参考/Elasticsearch/插件管理/UninstallPlugin.md)|调用UninstallPlugin，卸载已安装的预置插件。|
|日志查询|[ListSearchLog](/cn.zh-CN/API参考/Elasticsearch/日志查询/ListSearchLog.md)|调用ListSearchLog，查看实例日志。|
|安全配置|[TriggerNetwork](/cn.zh-CN/API参考/Elasticsearch/安全配置/TriggerNetwork.md)|调用TriggerNetwork，开启或关闭Elasticsearch、Kibana的公网或私网访问。|
|[UpdatePrivateNetworkWhiteIps](/cn.zh-CN/API参考/Elasticsearch/安全配置/UpdatePrivateNetworkWhiteIps.md)|调用UpdatePrivateNetworkWhiteIps，更新指定实例的VPC私网访问白名单。|
|[UpdatePublicWhiteIps](/cn.zh-CN/API参考/Elasticsearch/安全配置/UpdatePublicWhiteIps.md)|调用UpdatePublicWhiteIps，更新指定实例的公网地址访问白名单。|
|[UpdatePublicNetwork](/cn.zh-CN/API参考/Elasticsearch/安全配置/UpdatePublicNetwork.md)|调用UpdatePublicNetwork，开启或关闭指定实例的公网地址。|
|[ModifyWhiteIps](/cn.zh-CN/API参考/Elasticsearch/安全配置/ModifyWhiteIps.md)|调用ModifyWhiteIps，更新指定实例的访问白名单。|
|[UpdateAdminPassword](/cn.zh-CN/API参考/Elasticsearch/安全配置/UpdateAdminPassword.md)|调用UpdateAdminPassword，更新指定实例的elastic账号的密码。|
|[OpenHttps](/cn.zh-CN/API参考/Elasticsearch/安全配置/OpenHttps.md)|调用OpenHttps，开启HTTPS协议。开启前请确保您已购买协调节点。|
|[CloseHttps](/cn.zh-CN/API参考/Elasticsearch/安全配置/CloseHttps.md)|调用CloseHttps，关闭HTTPS协议。|
|[AddConnectableCluster](/cn.zh-CN/API参考/Elasticsearch/安全配置/AddConnectableCluster.md)|调用AddConnectableCluster，配置实例网络互通。|
|[DeleteConnectedCluster](/cn.zh-CN/API参考/Elasticsearch/安全配置/DeleteConnectedCluster.md)|调用DeleteConnectedCluster，移除互通实例。|
|[DescribeConnectableClusters](/cn.zh-CN/API参考/Elasticsearch/安全配置/DescribeConnectableClusters.md)|调用DescribeConnectableClusters，获取能够与当前实例进行网络互通的实例列表。不包括已经打通的实例。|
|[ListConnectedClusters](/cn.zh-CN/API参考/Elasticsearch/安全配置/ListConnectedClusters.md)|调用ListConnectedClusters，获取已经与当前实例进行了网络互通的实例列表。|
|数据备份|[CreateSnapshot](/cn.zh-CN/API参考/Elasticsearch/数据备份/CreateSnapshot.md)|调用CreateSnapshot，手动对集群进行快照备份。|
|[DescribeSnapshotSetting](/cn.zh-CN/API参考/Elasticsearch/数据备份/DescribeSnapshotSetting.md)|调用DescribeSnapshotSetting，获取集群的数据备份配置。|
|[UpdateSnapshotSetting](/cn.zh-CN/API参考/Elasticsearch/数据备份/UpdateSnapshotSetting.md)|调用UpdateSnapshotSetting，更新指定实例的数据备份配置。|
|[ListSnapshotReposByInstanceId](/cn.zh-CN/API参考/Elasticsearch/数据备份/ListSnapshotReposByInstanceId.md)|调用ListSnapshotReposByInstanceId，获取当前实例的跨集群OSS仓库设置列表。|
|[ListAlternativeSnapshotRepos](/cn.zh-CN/API参考/Elasticsearch/数据备份/ListAlternativeSnapshotRepos.md)|调用ListAlternativeSnapshotRepos，获取当前实例可添加的OSS引用仓库。|
|[AddSnapshotRepo](/cn.zh-CN/API参考/Elasticsearch/数据备份/AddSnapshotRepo.md)|调用AddSnapshotRepo，在设置跨集群OSS仓库时，创建引用仓库。|
|[DeleteSnapshotRepo](/cn.zh-CN/API参考/Elasticsearch/数据备份/DeleteSnapshotRepo.md)|调用DeleteSnapRepo，删除一个跨集群OSS引用仓库。|
|智能运维|[OpenDiagnosis](/cn.zh-CN/API参考/Elasticsearch/智能运维/OpenDiagnosis.md)|调用OpenDiagnosis，打开实例的智能运维功能。|
|[CloseDiagnosis](/cn.zh-CN/API参考/Elasticsearch/智能运维/CloseDiagnosis.md)|调用CloseDiagnosis，关闭实例的智能运维功能。|
|[ListDiagnoseReport](/cn.zh-CN/API参考/Elasticsearch/智能运维/ListDiagnoseReport.md)|调用ListDiagnoseReport，获取智能运维的历史报告。|
|[ListDiagnoseReportIds](/cn.zh-CN/API参考/Elasticsearch/智能运维/ListDiagnoseReportIds.md)|调用ListDiagnoseReportIds，获取智能运维历史报告的ID。|
|[DescribeDiagnoseReport](/cn.zh-CN/API参考/Elasticsearch/智能运维/DescribeDiagnoseReport.md)|调用DescribeDiagnoseReport，查看智能运维的历史报告。|
|[DescribeDiagnosisSettings](/cn.zh-CN/API参考/Elasticsearch/智能运维/DescribeDiagnosisSettings.md)|调用DescribeDiagnosisSettings，获取智能运维的场景设置。|
|[UpdateDiagnosisSettings](/cn.zh-CN/API参考/Elasticsearch/智能运维/UpdateDiagnosisSettings.md)|调用UpdateDiagnosisSettings，更新实例的智能运维场景设置。|

## Kibana

|API|描述|
|---|--|
|[DescribeKibanaSettings](/cn.zh-CN/API参考/Kibana/DescribeKibanaSettings.md)|调用DescribeKibanaSettings，获取Kibana配置。|
|[UpdateKibanaSettings](/cn.zh-CN/API参考/Kibana/UpdateKibanaSettings.md)|调用UpdateKibanaSettings，修改Kibana配置。目前仅支持修改Kibana语言配置。|
|[ListKibanaPlugins](/cn.zh-CN/API参考/Kibana/ListKibanaPlugins.md)|调用ListKibanaPlugins，获取Kibana插件列表。|
|[InstallKibanaSystemPlugin](/cn.zh-CN/API参考/Kibana/InstallKibanaSystemPlugin.md)|调用InstallKibanaSystemPlugin，安装Kibana预置插件。要求Kibana的规格为2核4G及以上。|
|[UninstallKibanaPlugin](/cn.zh-CN/API参考/Kibana/UninstallKibanaPlugin.md)|调用UninstallKibanaPlugin，卸载Kibana插件。|

## Logstash

|类别|API|描述|
|--|---|--|
|实例管理|[ListLogstash](/cn.zh-CN/API参考/Logstash/实例管理/ListLogstash.md)|调用ListLogstash，在列表中展示所有或指定Logstash实例的详细信息。|
|[DescribeLogstash](/cn.zh-CN/API参考/Logstash/实例管理/DescribeLogstash.md)|调用DescribeLogstash，查询指定实例的详细信息。|
|[UpdateLogstash](/cn.zh-CN/API参考/Logstash/实例管理/UpdateLogstash.md)|调用UpdateLogstash，修改指定实例的部分信息，例如节点数、配额、名称、硬盘大小等。|
|[RenewLogstash](/cn.zh-CN/API参考/Logstash/实例管理/RenewLogstash.md)|调用RenewLogstash，为实例续费。|
|[RestartLogstash](/cn.zh-CN/API参考/Logstash/实例管理/RestartLogstash.md)|调用RestartLogstash，重启指定实例。重启后，实例会进入生效中（activing）状态。|
|[UpdateLogstashDescription](/cn.zh-CN/API参考/Logstash/实例管理/UpdateLogstashDescription.md)|调用UpdateLogstashDescription，修改指定Logstash实例的名称。|
|[UpdateLogstashChargeType](/cn.zh-CN/API参考/Logstash/实例管理/UpdateLogstashChargeType.md)|调用UpdateLogstashChargeType，将按量付费的阿里云Logstash实例转换为包年包月实例。|
|[EstimatedLogstashRestartTime](/cn.zh-CN/API参考/Logstash/实例管理/EstimatedLogstashRestartTime.md)|调用EstimatedLogstashRestartTime，获取Logstash实例重启的预估时间。|
|[DeleteLogstash](/cn.zh-CN/API参考/Logstash/实例管理/DeleteLogstash.md)|调用DeleteLogstash，释放指定实例。|
|集群配置|[ListExtendfiles](/cn.zh-CN/API参考/Logstash/集群配置/ListExtendfiles.md)|调用ListExtendfiles，获取Logstash实例的扩展文件配置。|
|[UpdateLogstashSettings](/cn.zh-CN/API参考/Logstash/集群配置/UpdateLogstashSettings.md)|调用UpdateLogstashSettings，更新指定Logstash实例的配置。|
|集群监控|[ListAvailableEsInstanceIds](/cn.zh-CN/API参考/Logstash/集群监控/ListAvailableEsInstanceIds.md)|调用ListAvailableEsInstanceIds，在设置Logstash实例的X-Pack监控时，获取可用的Elasticsearch实例列表（具备X-Pack监控能力）。|
|[ValidateConnection](/cn.zh-CN/API参考/Logstash/集群监控/ValidateConnection.md)|调用ValidateConnection，在Logstash实例的监控报警配置中，验证提供X-Pack监控的Elasticsearch实例的联通性。|
|[UpdateXpackMonitorConfig](/cn.zh-CN/API参考/Logstash/集群监控/UpdateXpackMonitorConfig.md)|调用UpdateXpackMonitorConfig，更新Logstash实例的X-Pack监控报警配置。|
|[DescribeXpackMonitorConfig](/cn.zh-CN/API参考/Logstash/集群监控/DescribeXpackMonitorConfig.md)|调用DescribeXpackMonitorConfig，获取Logstash实例的X-Pack监控配置。|
|插件管理|[ListLogstashPlugins](/cn.zh-CN/API参考/Logstash/插件管理/ListLogstashPlugins.md)|调用ListLogstashPlugins，获取所有或指定插件的详细信息。|
|[InstallLogstashSystemPlugin](/cn.zh-CN/API参考/Logstash/插件管理/InstallLogstashSystemPlugin.md)|调用InstallLogstashSystemPlugin，安装插件。|
|[UninstallLogstashPlugin](/cn.zh-CN/API参考/Logstash/插件管理/UninstallLogstashPlugin.md)|调用UninstallLogstashPlugin，卸载已安装的插件。|
|日志查询|[ListLogstashLog](/cn.zh-CN/API参考/Logstash/日志查询/ListLogstashLog.md)|调用ListLogstashLog，查看Logstash实例的日志。|
|变更任务管理|[InterruptLogstashTask](/cn.zh-CN/API参考/Logstash/变更任务管理/InterruptLogstashTask.md)|调用InterruptLogstashTask，中断实例变更任务。中断后，实例会进入中断中（suspended）状态。|
|[ResumeLogstashTask](/cn.zh-CN/API参考/Logstash/变更任务管理/ResumeLogstashTask.md)|调用ResumeLogstashTask，恢复实例的变更中断任务。恢复后实例会进入生效中（activating）状态。|
|管道管理|[CreatePipelines](/cn.zh-CN/API参考/Logstash/管道管理/CreatePipelines.md)|调用CreatePipelines，创建Logstash管道。|
|[ListPipeline](/cn.zh-CN/API参考/Logstash/管道管理/ListPipeline.md)|调用ListPipeline，获取Logstash实例的管道列表。|
|[DescribePipeline](/cn.zh-CN/API参考/Logstash/管道管理/DescribePipeline.md)|调用DescribePipeline，获取Logstash实例的管道信息。|
|[UpdatePipelines](/cn.zh-CN/API参考/Logstash/管道管理/UpdatePipelines.md)|调用UpdatePipelines，更新Logstash管道信息。|
|[RunPipelines](/cn.zh-CN/API参考/Logstash/管道管理/RunPipelines.md)|调用RunPipelines，立即部署Logstash管道。|
|[StopPipelines](/cn.zh-CN/API参考/Logstash/管道管理/StopPipelines.md)|调用StopPipelines，停止运行Logstash管道。|
|[UpdatePipelineManagementConfig](/cn.zh-CN/API参考/Logstash/管道管理/UpdatePipelineManagementConfig.md)|调用UpdatePipelineManagementConfig，更新Logstash管道管理方式。|
|[DescribePipelineManagementConfig](/cn.zh-CN/API参考/Logstash/管道管理/DescribePipelineManagementConfig.md)|调用DescribePipelineManagementConfig，获取Logstash管道管理配置。|
|[DeletePipelines](/cn.zh-CN/API参考/Logstash/管道管理/DeletePipelines.md)|调用DeletePipelines，删除指定的Logstash管道。|

## Beats

|API|描述|
|---|--|
|[t1950927.md\#]()|调用DescribeKibanaSettings，获取Kibana配置。|
|[ReinstallCollector](/cn.zh-CN/API参考/Beats/ReinstallCollector.md)|调用ReinstallCollector，重试安装在创建时没有安装成功的采集器。|
|[ListCollectors](/cn.zh-CN/API参考/Beats/ListCollectors.md)|调用ListCollectors，获取采集器列表信息。|
|[UpdateCollectorName](/cn.zh-CN/API参考/Beats/UpdateCollectorName.md)|调用UpdateCollectorName，修改采集器名称。|
|[StartCollector](/cn.zh-CN/API参考/Beats/StartCollector.md)|调用StartCollector，启动采集器。|
|[StopCollector](/cn.zh-CN/API参考/Beats/StopCollector.md)|调用StopCollector，停止运行中的采集器。|
|[DeleteCollector](/cn.zh-CN/API参考/Beats/DeleteCollector.md)|调用DeleteCollector，删除采集器。|

