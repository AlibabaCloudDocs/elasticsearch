---
keyword: [filebeat.yml配置, metricbeat.yml配置, auditbeat.yml配置, heartbeat.yml配置]
---

# 采集器YML配置

通过采集器YML配置，您可以根据需求修改对应的配置，并启用该配置，完成数据采集任务。本文介绍采集器YML文件的配置方法和配置参数详情。

## 前提条件

创建阿里云Elasticsearch（简称ES）实例，并开启实例的**自动创建索引**功能。创建实例的具体步骤请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

出于安全考虑，阿里云ES默认不允许**自动创建索引**。但是Beats目前依赖该功能，因此如果**采集器Output**选择为**Elasticsearch**，需要开启**自动创建索引**功能，详情请参见[开启自动创建索引](/cn.zh-CN/Elasticsearch/快速访问与配置.md)。

**说明：** Beats中包含很多module供您使用，阿里云Beats未直接提供这些module的单独配置，如果需要使用这些module，请直接在相应的主配置文件中配置。例如，在Metricbeat中启用system module，可以在metricbeat.yml配置中添加如下脚本。

```
metricbeat.modules:
- module: system
metricsets: ["diskio","network"]
diskio.include_devices: []
period: 1s
```

## Filebeat配置

通过在**filebeat.yml**中指定`filebeat.inputs`，来决定如何查找或处理输入数据源。简单的input配置如下。

![FileBeat配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1342387951/p76916.png)

```
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /opt/test/logs/t1.log
    - /opt/test/logs/t2/*
  fields:
    alilogtype: usercenter_serverlog
```

**说明：**

-   如果在[采集ECS服务日志](/cn.zh-CN/Beats/采集ECS服务日志.md)时，指定了**采集器Output**，那么不需要再在**采集器YML配置**中单独设置Output，否则会提示ECS采集器安装错误。
-   每个输入源都以`-`开头，可以指定多个`-`，代表多个输入源。

|配置|说明|
|--|--|
|`type`|输入类型。默认是`log`类型，同时还支持`stdin`、`redis`、`tcp`、`syslog`等类型。|
|`paths`|指定要监控的日志。可以指定具体的文件或者目录，指定的文件或目录会被映射进Docker目录中。|
|`enabled`|设置配置是否生效。`true`表示生效，`false`表示不生效。|
|`fields`|指定可选字段。在该字段下，空格缩进两格填写需要添加的字段。例如，设置为`alilogtype: usercenter_serverlog`，表示在输出的每条日志中加入该字段，用于标识该日志源的类别，在传输到下一层Logstash时，可以根据该字段类别，对日志进行分类处理。|

更多配置详情请参见[官方Log input文档](https://www.elastic.co/guide/en/beats/filebeat/6.7/filebeat-input-log.html#input-paths)。

## Metricbeat配置

Metricbeat能够以一种轻量型的方式，输送各种系统和服务统计数据。通过在**metricbeat.yml**中指定`metricbeat.modules`，来指定`module`配置。

![MetricBeat配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1342387951/p76949.png)

```
metricbeat.modules:
- module: system
  metricsets: ["diskio","network"]
  enabled: true
  hosts: ["http://XX.XX.XX.XX/"]
  period: 10s
  fields:
    dc: west
  tags: ["tag"]
```

**说明：** 如果在[采集ECS服务日志](/cn.zh-CN/Beats/采集ECS服务日志.md)时，指定了**采集器Output**，那么不需要再在**采集器YML配置**中单独设置Output，否则会提示ECS采集器安装错误。

|配置|说明|
|--|--|
|`module`|要运行的模块的名称。支持的模块及相关说明请参见[Modules](https://www.elastic.co/guide/en/beats/metricbeat/6.7/metricbeat-modules.html)。|
|`metricsets`|指定要执行的metricsets列表。更多metricsets列表请参见[Modules](https://www.elastic.co/guide/en/beats/metricbeat/6.7/metricbeat-modules.html)。|
|`enabled`|设置配置是否生效。`true`表示生效，`false`表示不生效。|
|`period`|metricsets执行的频率。如果无法访问系统，metricbeat将为每个时间段返回一个错误信息。|
|`hosts`|可选，获取主机列表信息。|
|`fields`|指定可选字段。与metricset事件一起发送的字段。|
|`tags`|可选，与metricset一起发送的tag列表。|

更多配置详情请参见[官方Metricbeat配置文档](https://www.elastic.co/guide/en/beats/metricbeat/6.7/configuration-metricbeat.html)。

## Heartbeat配置

Heartbeat能够以一种轻量型的方式，安装在远程服务器，定期检查服务的状态并确定是否可用。与Metricbeat不同，Metricbeat检测服务是启动还是关闭状态，Heartbeat检测服务是否可访问。

通过在**heartbeat.yml**中指定`heartbeat.monitors`，来指定监控的服务。

**说明：** Heartbeat只需要配置监控的服务即可，为保证可用性，建议至少部署2台云服务器ECS。

![HeartBeat配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1342387951/p76955.png)

```
heartbeat.monitors:
- type: http
  name: ecs_monitor
  enabled: true
  urls: ["http://localhost:9200"]
  schedule: '@every 5s'
  fields:
    dc: west
```

**说明：** 如果在[采集ECS服务日志](/cn.zh-CN/Beats/采集ECS服务日志.md)时，指定了**采集器Output**，那么不需要再在**采集器YML配置**中单独设置Output，否则会提示ECS采集器安装错误。

|配置|说明|
|--|--|
|`type`|monitor类型。支持`icmp`、`tcp`、`http`。|
|`name`|monitor名称。该值显示在monitor字段下的[Exported fields](https://www.elastic.co/guide/en/beats/heartbeat/6.7/exported-fields.html)中，作为job name，`type`字段作为job type。|
|`enabled`|设置配置是否生效。`true`表示生效，`false`表示不生效。|
|`urls`|可选，用来连通的服务器列表。|
|`schedule`|指定任务计划。设置为`@every 5s`，表示从启动Heartbeat开始，每5秒运行一次任务；设置为`*/5 * * * * * *`，表示每5秒运行一次任务。|
|`fields`|指定可选字段。将该字段添加到output中，作为附加信息。|

更多配置详情请参见[官方Heartbeat配置文档](https://www.elastic.co/guide/en/beats/heartbeat/6.7/configuration-heartbeat-options.html)。

## Auditbeat配置

Auditbeat是轻量型的审计日志采集器，可以收集Linux审计框架的数据，监控文件的完整性。Auditbeat能够组合相关消息到一个事件里，生成标准的结构化数据，方便分析，而且能够与Logstash、Elasticsearch和Kibana无缝集成。

**说明：** Auditbeat依赖于操作系统的Linux Audit Framework，要求OS Kernel版本至少为3.14。且保证Auditd服务为stop状态（可使用`service auditd status`命令查看）。

通过在**auditbeat.yml**中指定`auditbeat.modules`，来配置Auditbeat采集器。**auditbeat.yml**主要分为两部分，一部分为模块，另一部分为输出。 启用特定模块，需要在**auditbeat.yml**中添加特定参数，如下示例展示`auditd`和`file_integrity`配置。

```
auditbeat.modules:
- module: auditd
  audit_rules: |
    -w /etc/passwd -p wa -k identity
    -a always,exit -F arch=b32 -S open,create,truncate,ftruncate,openat,open_by_handle_at -F exit=-EPERM -k access
- module: file_integrity
  paths:
  - /bin
  - /usr/bin
  - /sbin
  - /usr/sbin
  - /etc
```

**说明：** 如果在[采集ECS服务日志](/cn.zh-CN/Beats/采集ECS服务日志.md)时，指定了**采集器Output**，那么不需要再在**采集器YML配置**中单独设置Output，否则会提示ECS采集器安装错误。

更多**auditbeat.yml**配置说明请参见[官方Auditbeat配置文档](https://www.elastic.co/guide/en/beats/auditbeat/6.7/auditbeat-configuration.html)；配置模块（`module`）详情请参见[Modules](https://www.elastic.co/guide/en/beats/auditbeat/6.7/auditbeat-modules.html)。

