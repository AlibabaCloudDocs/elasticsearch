---
keyword: Beats收集指标信息
---

# 通过自建Metricbeat收集系统指标信息

当您需要查看并分析一台机器的指标信息时，可以使用Metricbeat采集该机器的指标信息，推送到阿里云Elasticsearch上。然后在Kibana中搜索分析，并生成对应的Dashborard。本文以Mac电脑为例，介绍具体的实现方法。

您已完成以下操作：

-   创建阿里云Elasticsearch实例。

    具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

    **说明：** 如果您需要通过内网地址来访问阿里云Elasticsearch实例，还需要购买一个与实例在相同地域和可用区，以及相同专有网络下的云服务器ECS实例。具体操作，请参见[使用向导创建实例](/cn.zh-CN/实例/创建实例/使用向导创建实例.md)。

-   下载Metricbeat：
    -   MAC系统的Metricbeat安装包[下载地址](https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.0-darwin-x86_64.tar.gz)。
    -   32位Linux系统的Metricbeat安装包[下载地址](https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.0-linux-x86.tar.gz)。
    -   64位Linux系统的Metricbeat安装包[下载地址](https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.0-linux-x86_64.tar.gz)。
    -   32位Windows系统的Metricbeat安装包[下载地址](https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.0-windows-x86.zip)。
    -   64位Windows系统的Metricbeat安装包[下载地址](https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.0-windows-x86_64.zip)。

Beats是一个集合了多种单一用途的数据采集器的平台，这些采集器安装后可用作轻量型代理，从成百上千或成千上万台机器向Logstash或Elasticsearch实例发送数据。

Metricbeat是一个轻量级的指标采集器，可以从系统和服务中收集指标。从CPU到内存，从Redis到Nginx，Metricbeat能够以一种轻量型的方式采集各种系统和服务的统计数据。

## 操作流程

1.  [配置阿里云Elasticsearch](#section_llm_6tp_v57)

2.  [配置Metricbeat](#section_weo_gpf_ixo)

3.  [在Kibana中查看Dashboard](#section_863_9xl_fyc)

    **说明：** 您也可以参考本案例的操作流程，使用Metricbeat采集一台Linux系统或Windows系统电脑的指标信息，并推送到阿里云Elasticsearch上。


## 配置阿里云Elasticsearch

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  在顶部菜单栏处，选择资源组和地域，然后在**实例列表**中单击目标实例ID。

4.  在左侧导航栏，单击**安全配置**。

5.  打开**公网地址**开关，待配置生效后，单击**公网地址访问白名单**右侧的**修改**，将您MAC机器对外的公网IP地址配置到公网地址访问白名单中。

    ![配置ES公网地址访问白名单](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7934659951/p40014.png)

    **说明：** 如果您使用的是WIFI等网络，需要将公网出口的跳板机IP地址配置进去。如果获取不到，建议配置`0.0.0.0/1,128.0.0.0/1`来开放尽可能多的IP地址（本篇以此为例）。需要注意这个配置将导致您的阿里云Elasticsearch服务完全暴露在公网中，需要先评估下是否可以接受这个风险。

6.  在左侧导航栏，单击**基本信息**。在**基本信息**区域，获取Elasticsearch实例的**公网地址**备用。

    ![获取ES实例的公网地址](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7934659951/p40016.png)

7.  在左侧导航栏，单击**ES集群配置**。在**YML文件配置**区域，单击右侧的**修改配置**，将**自动创建索引**设置为**允许自动创建索引**。

    ![允许自动创建索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3190152161/p40017.png)

    **警告：** 此配置需要重启Elasticsearch实例才能生效，为保证您的业务不受影响，请确认后再进行后续操作。

8.  勾选**该操作会重启实例，请确认后操作**，单击**确定**。

    重启过程中，可在[任务列表](/cn.zh-CN/Elasticsearch/实例管理/查看实例任务进度详情.md)查看重启进度。重启完成后，即可完成实例的配置。


## 配置Metricbeat

1.  将您下载的Metricbeat安装包解压缩，并进入Metricbeat文件夹。

    ![进入Metricbeat文件夹](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7934659951/p40018.png)

2.  打开metricbeat.yml文件，定位到`Elasticsearch output`部分，取消对应内容的注释。

    ![编辑metricbeat.yml文件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7934659951/p40019.png)

    |参数|说明|
    |--|--|
    |hosts|Elasticsearch实例的公网或内网地址（本文以公网地址为例）。|
    |protocol|需要配置为http。|
    |username|默认是elastic。|
    |password|对应用户的密码。elastic用户的密码在创建实例时设定，如果忘记可重置。重置密码的注意事项和操作步骤，请参见[重置实例访问密码](/cn.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|

3.  执行以下命令，启动Metricbeat。

    ```
    ./metricbeat -e -c metricbeat.yml
    ```

    ![Metricbeat启动成功状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7934659951/p40020.png)

    启动成功后，Metricbeat开始向Elasticsearch实例推送数据。


## 在Kibana中查看Dashboard

1.  登录目标Elasticsearch实例的Kibana控制台。

    具体操作，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Management**，按照以下步骤创建一个索引模式。

    **说明：** 如果已经创建了索引模式，可忽略此步骤。

    1.  在**Management**页面，单击**Kibana**区域中的**Index Patterns**。

    2.  在**Create index pattern**页面，输入索引模式名称（待查询的索引名称）。

    3.  单击**Next step**。

        ![创建索引模式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7934659951/p94699.png)

    4.  单击**Create index pattern**。

3.  在左侧导航栏，单击**Dashboard**。

4.  在**Dashboard**页面查看相关信息。

    -   各类相关指标列表

        ![各类相关指标列表](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7934659951/p40021.png)

    -   单击**Metricbeat-cpu**，查看CPU指标信息

        ![Metricbeat-cpu指标信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8934659951/p40022.png)

        **说明：** 您可以将数据定义为5s刷新一次，然后生成对应的报表，并接入WebHook进行异常告警。


## 相关文档

[借助Beats快速搭建可视化运维系统](https://yq.aliyun.com/articles/618611)

