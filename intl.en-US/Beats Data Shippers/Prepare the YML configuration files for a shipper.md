---
keyword: [filebeat.yml configuration, metricbeat.yml configuration, auditbeat.yml configuration, heartbeat.yml configuration]
---

# Prepare the YML configuration files for a shipper

You can modify and enable the YML configuration of a shipper to complete specific data collection tasks. This topic discusses specific parameters in the YML configuration files and describes how to modify the YML configuration.

## Prerequisites

An Alibaba Cloud Elasticsearch cluster is created, and the **Auto Indexing** feature is enabled for the cluster. For more information about how to create an Elasticsearch cluster, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

For security purposes, Alibaba Cloud Elasticsearch disables the **Auto Indexing** feature by default. However, Beats depends on this feature. If you select **Elasticsearch** for **Output** when you install a shipper, you must enable the **Auto Indexing** feature. For more information, see [t1605396.md\#section\_pcn\_1xy\_1l2](/intl.en-US/Elasticsearch Instances Management/Access and configure an Elasticsearch cluster.md).

**Note:** Open source Beats provides multiple modules, but Alibaba Cloud Beats does not provide separate configuration for these modules. If you want to use them, you must configure them in the configuration files of different shippers. For example, if you want to enable the system module in a Metricbeat shipper, add the following script to metricbeat.yml:

```
metricbeat.modules:
- module: system
metricsets: ["diskio","network"]
diskio.include_devices: []
period: 1s
```

## Filebeat configuration

You can specify `filebeat.inputs` in **filebeat.yml** to determine how to search for or handle input data sources. The following figure shows an example of a simple input configuration.

![Filebeat configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9992640951/p76916.png)

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

**Note:**

-   If you specify **Output** when you [install a shipper](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md), you do not need to specify it again in **Shipper YML Configuration**. Otherwise, the system prompts a shipper installation error.
-   Each input data source starts with a hyphen \(`-`\). You can use multiple hyphens to specify multiple input data sources.

|Parameter|Description|
|---------|-----------|
|`type`|The input type. Examples of valid values: `stdin`, `redis`, `tcp`, and `syslog`. Default value: `log`.|
|`paths`|The paths of the logs you want to monitor. You can specify a file or a directory to map to Docker.|
|`enabled`|Specifies whether the configuration takes effect. The value `true` indicates that the configuration takes effect. The value `false` indicates that the configuration does not take effect.|
|`fields`|Optional. Below this parameter, you can indent with two spaces to add fields. For example, enter `alilogtype: usercenter_serverlog` to add this field to each output log to identify the type of the log source. If logs are shipped to Logstash, they can be classified and processed based on this field.|

For more information, see [Log input](https://www.elastic.co/guide/en/beats/filebeat/6.7/filebeat-input-log.html#input-paths) in the open source Filebeat documentation.

## Metricbeat configuration

Metricbeat delivers system and service statistics in a lightweight manner. You can specify `metricbeat.modules` in **metricbeat.yml** to configure a `module`.

![Metricbeat configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6544404061/p76949.png)

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

**Note:** If you specify **Output** when you [install a shipper](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md), you do not need to specify it again in **Shipper YML Configuration**. Otherwise, the system prompts a shipper installation error.

|Parameter|Description|
|---------|-----------|
|`module`|The name of the module you want to run. For more information about supported modules, see [Modules](https://www.elastic.co/guide/en/beats/metricbeat/6.7/metricbeat-modules.html).|
|`metricsets`|Specifies the metricsets you want to execute. For more information about metricsets, see [Modules](https://www.elastic.co/guide/en/beats/metricbeat/6.7/metricbeat-modules.html).|
|`enabled`|Specifies whether the configuration takes effect. The value `true` indicates that the configuration takes effect. The value `false` indicates that the configuration does not take effect.|
|`period`|Specifies how often the metricsets are executed. If the system is inaccessible, Metricbeat returns an error for each period.|
|`hosts`|Optional. This parameter specifies the hosts from which you want to obtain information.|
|`fields`|Optional. This parameter specifies the fields that are sent with the metricset event.|
|`tags`|Optional. This parameter specifies the tags that are sent with the metricset event.|

For more information, see open source [Metricbeat documentation](https://www.elastic.co/guide/en/beats/metricbeat/6.7/configuration-metricbeat.html).

## Heartbeat configuration

Heartbeat can be installed on a remote server in a lightweight manner. You can use Heartbeat to periodically check the status of your services and determine whether they are available. Unlike Metricbeat, Heartbeat checks whether your services are available but Metricbeat checks whether your services are running.

You can specify `heartbeat.monitors` in **heartbeat.yml** to specify the services you want to monitor.

**Note:** You can configure only the services that you want to monitor for Heartbeat. To ensure the availability of Heartbeat, we recommend that you deploy at least two Elastic Compute Service \(ECS\) instances.

![Heartbeat configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9992640951/p76955.png)

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

**Note:** If you specify **Output** when you [install a shipper](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md), you do not need to specify it again in **Shipper YML Configuration**. Otherwise, the system prompts a shipper installation error.

|Parameter|Description|
|---------|-----------|
|`type`|The monitor type. Valid values: `icmp`, `tcp`, and `http`.|
|`name`|The monitor name. This value appears in [Exported fields](https://www.elastic.co/guide/en/beats/heartbeat/6.7/exported-fields.html) of the monitor field and is used as the job name. The `type` field is used as the job type.|
|`enabled`|Specifies whether the configuration takes effect. The value `true` indicates that the configuration takes effect. The value `false` indicates that the configuration does not take effect.|
|`urls`|Optional. This parameter specifies the servers to which you want to connect.|
|`schedule`|The task schedule. If you set the value to `@every 5s`, the system runs the task every five seconds from the time Heartbeat is started. If you set the value to `*/5 * * * * * *`, the system runs the task every five seconds.|
|`fields`|Optional. You can add the fields to output as additional information.|

For more information, see open source [Heartbeat documentation](https://www.elastic.co/guide/en/beats/heartbeat/6.7/configuration-heartbeat-options.html).

## Auditbeat configuration

Auditbeat is a lightweight shipper that collects audit logs from the Linux audit framework and monitors file integrity. Auditbeat combines relevant messages into an event to generate structured data for analytics. It can also be seamlessly integrated with Logstash, Elasticsearch, and Kibana.

**Note:** Auditbeat is based on the Linux audit framework and requires an OS kernel version of 3.14 or later. The Auditd service must be in the stop state. You can run the `service auditd status` command to query its status.

You can specify `auditbeat.modules` in **auditbeat.yml** to configure the Auditbeat shipper. **auditbeat.yml** consists of two parts: module and output. If you want to enable a module, you must add specific parameters to **auditbeat.yml**. The following configurations use the `auditd` and `file_integrity` modules as examples:

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

**Note:** If you specify **Output** when you [install a shipper](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md), you do not need to specify it again in **Shipper YML Configuration**. Otherwise, the system prompts a shipper installation error.

For more information about **auditbeat.yml** configuration, see [Step 2: Configure Auditbeat](https://www.elastic.co/guide/en/beats/auditbeat/6.7/auditbeat-configuration.html) in the open source Auditbeat documentation. For more information about `module` configuration, see [Modules](https://www.elastic.co/guide/en/beats/auditbeat/6.7/auditbeat-modules.html).

