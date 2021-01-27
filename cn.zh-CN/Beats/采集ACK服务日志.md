---
keyword: [Filebeat采集ACK服务日志, Filebeat采集K8S日志]
---

# 采集ACK服务日志

阿里云Filebeat支持采集容器服务Kubernetes版ACK（Container Service for Kubernetes）日志，并将采集的日志输出到阿里云Elasticsearch中，进行分析展示。本文介绍如何配置ACK服务日志采集。

-   创建阿里云Elasticsearch实例。

    具体操作，请参见[创建实例](/cn.zh-CN/Elasticsearch/实例管理/创建实例.md)。

-   自定义自动创建索引。

    为避免索引滚动别名与索引名冲突，建议仅开启filebeat-\*索引名，可配置为**+.\*,+filebeat-\*,-\***。具体操作，请参见[配置YML参数](/cn.zh-CN/Elasticsearch/集群配置/配置YML参数.md)。

    ![自定义自动创建索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4241651161/p231744.png)

    **说明：** 在[配置索引生命周期管理](#step_iih_doy_mcu)时，如果开启了索引**滚动更新**，为避免滚动更新后的别名与索引名冲突，需要确保关闭自动创建索引；如果未开启索引**滚动更新**，则需要开启自动创建索引。本文建议您设置自定义自动创建索引。

-   RAM用户进行相关操作时，需要为该用户授予Beats操作权限和ACK操作权限。

    详细信息，请参见[创建自定义权限策略](/cn.zh-CN/访问控制/创建自定义权限策略.md)和[配置子账号RBAC权限](/cn.zh-CN/Kubernetes集群用户指南/授权管理/配置子账号RBAC权限.md)。

-   创建ACK集群，并运行Pod服务。本文以Nginx容器为例。

    具体操作，请参见[创建Kubernetes托管版集群](/cn.zh-CN/Kubernetes集群用户指南/集群管理/创建集群/创建Kubernetes托管版集群.md)。


## 操作步骤

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在顶部菜单栏处，选择地域。然后在左侧导航栏，单击**Beats数据采集中心**。

    **说明：** 首次进入**Beats数据采集中心**，需要根据页面提示进行授权确认。

3.  在**创建采集器**区域，将鼠标移至**Filebeat**上，单击**ACK日志**。

4.  在**选择目标ES集群**配置向导中，配置采集器信息。完成后，单击**下一步**。

    ![选择目标ES集群](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4241651161/p231821.png)

    |参数|说明|
    |--|--|
    |**采集器名称**|自定义输入采集器的名称。长度为1~30个字符，以大小写字母开头，可以包含字母、数字、下划线（\_）或连字符（-）。|
    |**安装版本**|目前Filebeat只支持**6.8.13**版本。|
    |**采集器Output**|Filebeat采集内容的输出地址。直接关联已经创建的阿里云Elasticsearch实例。访问协议需要与所选Elasticsearch实例保持一致。|
    |**用户名密码**|访问阿里云Elasticsearch实例的用户名和密码。用户名默认为elastic，密码在创建实例时设定，如果忘记可重置。重置密码的注意事项和操作步骤，请参见[重置实例访问密码](/cn.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|
    |**启用Monitoring**|用来监控Filebeat的相关指标。**采集器Output**选择**Elasticsearch**，Monitor默认使用和**采集器Output**相同的阿里云Elasticsearch实例。|
    |**启用Kibana Dashboard**|用来配置默认的Kibana Dashboard。由于阿里云Kibana配置在专有网络内，因此需要在Kibana配置页面开通Kibana私网访问功能。具体操作，请参见[配置Kibana公网或私网访问白名单](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/配置Kibana公网或私网访问白名单.md)。|

5.  在**设置采集目标**配置向导中，配置采集目标信息。

    1.  选择**ACK目标集群**。

        **说明：** 请选择与Elasticsearch实例在同一专有网络、Running状态和非边缘托管版（[ACK@Edge概述](/cn.zh-CN/边缘容器服务ACK@Edge用户指南/ACK@Edge概述.md)）的ACK集群。

    2.  根据提示，单击**安装**，安装采集依赖的ES-operator。

        没有**安装**按钮，表示已经安装。安装后，**安装**按钮消失，代表安装成功。

    3.  单击**+创建采集目标**，配置采集目标信息（支持多个）。

        ![创建采集目标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4241651161/p231833.png)

        |参数|说明|
        |--|--|
        |**采集目标名称**|创建多个采集目标时，名称不可重复。|
        |**命名空间**|选择采集Pod所在的命名空间。如果创建Pod时，未指定命名空间，则默认是default。|
        |**Pod Label**|添加Pod标签。添加多个标签时，多个标签之间为逻辑与关系。**说明：** 删除Pod标签时，需要至少存在两个标签。 |
        |**容器名称**|指定完整的容器名称。不填时，Filebeat会采集命名空间下符合Pod标签的所有容器。|

        **说明：** 获取Pod相关信息（命名空间、标签、名称）的具体操作，请参见[查看容器组（Pod）](/cn.zh-CN/Kubernetes集群用户指南/应用管理/查看容器组（Pod）.md)。

    4.  单击**下一步**。

6.  在**日志采集配置**配置向导中，单击**+添加日志采集配置**，配置日志采集信息（支持多个）。完成后，单击**下一步**。

    ![日志采集配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6638651161/p232001.png)

    |参数|说明|
    |--|--|
    |**日志名称**|每个采集目标支持定义多个采集配置，每个采集配置对应一个日志名称，不可重复。日志名称可作为索引名的一部分，用于后续输出使用。|
    |**采集器配置**|使用Docker容器日志采集通用模板。采集配置集成[Autodiscover](https://www.elastic.co/guide/en/beats/filebeat/7.10/configuration-autodiscover.html#_kubernetes)事件变量，支持[Docker input](https://www.elastic.co/guide/en/beats/filebeat/6.8/filebeat-input-docker.html#filebeat-input-docker)，具体说明如下：    -   type：输入类型。采集容器日志时为docker，不同的数据采集支持不同的类型。详细信息，请参见[Configure inputs](https://www.elastic.co/guide/en/beats/filebeat/7.10/configuration-filebeat-options.html)。
    -   combine\_partial：合并多行日志。详细信息，请参见[Docker input（combine\_partial）](https://www.elastic.co/guide/en/beats/filebeat/7.10/filebeat-input-docker.html#_combine_partial)。
    -   containers.ids：Docker容器日志的ID列表。详细信息，请参见[Docker input（containers.ids）](https://www.elastic.co/guide/en/beats/filebeat/6.8/filebeat-input-docker.html#config-container-ids)。
    -   fileds.k8s\_container\_name：在采集器的输出信息中，添加k8s\_container\_name字段，引用变量$\{data.kubernetes.container.name\}。

**说明：** Docker容器配置集成了[Filebeat Autodiscover](https://www.elastic.co/guide/en/beats/filebeat/7.10/configuration-autodiscover.html#_kubernetes)，配置模板可引用[Autodiscover](https://www.elastic.co/guide/en/beats/filebeat/7.10/configuration-autodiscover.html#_kubernetes)事件变量。

    -   fileds.k8s\_node\_name：在采集器的输出信息中，添加k8s\_node\_name字段，引用变量$\{data.kubernetes.node.name\}。
    -   fileds.k8s\_pod：在采集器的输出信息中，添加k8s\_pod字段，引用变量$\{data.kubernetes.pod.name\}。
    -   fileds.k8s\_pod\_namespace：在采集器的输出信息中，添加k8s\_pod\_namespace字段，引用变量$\{data.kubernetes.namespace\}。
    -   fields\_under\_root：设置为true，fileds存储在输出文档的顶级位置。详细信息，请参见[Docker input（fields\_under\_root）](https://www.elastic.co/guide/en/beats/filebeat/6.8/filebeat-input-docker.html#fields-under-root-docker)。
**说明：**

    -   如果通用模板无法满足需求，建议您参见[FileBeat Docker input](https://www.elastic.co/guide/en/beats/filebeat/6.8/filebeat-input-docker.html#fields-under-root-docker)修改。
    -   一个采集器配置仅支持一个Docker输出。如果需要使用多个配置，请单击**添加日志采集配置**。 |

7.  在**索引存储管理**配置向导中，根据需求开启并配置索引存储管理功能。

    开启索引存储管理后，单击**+添加索引管理策略**，按照以下说明配置策略（支持多个）。

    ![索引存储管理](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4241651161/p231865.png)

    |参数|说明|
    |--|--|
    |**策略名称**|每个采集目标支持定义多个采集配置，每个采集配置对应一个日志名称，日志名称可作为索引名用于后续输出使用。|
    |**选择目标日志**|选择待关联的目标日志，至少选择一个。一个策略可选择多个目标日志，每个日志仅可被一个日志管理策略选择。|
    |**资源大小限制**|索引实际占用磁盘空间达到资源限制时（包括副本），将删除过旧数据，以保证不超过该资源大小限制。|
    |**生命周期管理**|是否开启索引生命周期管理。生命周期管理可以实现数据节点冷热分离、过期自动删除数据等。详细信息，请参见[冷热分离与索引生命周期管理](/cn.zh-CN/最佳实践/Elasticsearch应用/集群管理/冷热分离与索引生命周期管理.md)和[Managing the index lifecycle](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/index-lifecycle-management.html)。**说明：**

    -   开启滚动更新，Filebeat会将数据写入名称为<日志名称\>-<日期\>-<序号\>的索引下，例如log-web-2021.01.22-000001。
    -   不开启滚动更新，Filebeat会将数据写入名称为filebeat-<日志名称\>-<日期\>的索引下。 |

8.  单击**启动**。

    启动后，您可在采集器列表中查看对应的采集器。启动成功后，您还可以完成以下操作。

    |操作|说明|
    |--|--|
    |**配置预览**|可查看采集器Output、ACK目标集群及采集任务名称等信息，不支持修改这些信息。|
    |**修改详情配置**|可修改采集目标、日志采集配置和索引存储管理策略。|
    |**更多**|可启动、停止、重启、删除采集任务，并支持查看Dashboard和Helm状态。|


