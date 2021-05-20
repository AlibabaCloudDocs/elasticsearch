---
keyword: [Auditbeat采集器, Auditbeat收集审计框架数据, Auditbeat监控系统文件更改]
---

# 通过Auditbeat收集系统审计数据并监控文件更改

本文介绍如何通过阿里云Auditbeat收集Linux系统的审计框架数据，监控系统文件的更改情况，并生成可视化图表。

Auditbeat是轻量型的审计日志采集器，可以收集Linux审计框架的数据，并监控文件完整性。例如使用Auditbeat从Linux Audit Framework收集和集中审核事件，以及检测对关键文件（如二进制文件和配置文件）的更改，并确定潜在的安全策略冲突，生成标准的结构化数据，方便分析。而且Auditbeat能够与Logstash、Elasticsearch和Kibana无缝集成。

Auditbeat支持两种模块：

-   Auditd

    Auditd模块接收来自Linux审计框架的审计事件，审计框架是Linux内核的一部分。Auditd模块可以建立对内核的订阅，以便在事件发生时接收它们，详情请参见[官方Auditd文档](https://www.elastic.co/guide/en/beats/auditbeat/6.8/auditbeat-module-auditd.html)。

    **说明：** 在启用Auditd模块的情况下运行Auditbeat时，您可能会发现其他监控工具会干扰Auditbeat。例如，如果注册了另一个进程（例如auditd）来从Linux Audit Framework接收数据，则可能会遇到错误。此时可以使用`service auditd stop`命令停止该进程。

-   File Integrity

    File Integrity模块可以实时监控指定目录下的文件的更改情况。如果要在Linux系统中使用File Integrity，请确保Linux内核支持inotify（2.6.13以上内核均已安装inotify），详情请参见[官方File Integrity文档](https://www.elastic.co/guide/en/beats/auditbeat/6.8/auditbeat-module-file_integrity.html)。

    **说明：** 目前官方Auditbeat System模块还处于实验阶段，将来的版本中可能删除或者更改，建议不要使用。更多模块详情请参见[modules](https://www.elastic.co/guide/en/beats/auditbeat/6.8/auditbeat-modules.html)。


## 前提条件

您已完成以下操作：

-   创建阿里云Elasticsearch实例。

    详情请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

-   开启阿里云ES实例的**自动创建索引**功能。

    出于安全考虑，阿里云ES默认不允许**自动创建索引**。但是Beats目前依赖该功能，因此如果**采集器Output**选择为**Elasticsearch**，需要开启**自动创建索引**功能，详情请参见[开启自动创建索引](/cn.zh-CN/Elasticsearch/快速访问与配置.md)。

-   创建阿里云ECS实例，且该ECS实例与阿里云ES实例处于同一专有网络VPC（Virtual Private Cloud）下。

    详情请参见[使用向导创建实例](/cn.zh-CN/实例/创建实例/使用向导创建实例.md)。

    **说明：** Beats目前仅支持Aliyun Linux、RedHat和CentOS这三种操作系统。

-   在目标ECS实例上安装云助手和Docker服务。

    详情请参见[安装云助手客户端](/cn.zh-CN/运维与监控/云助手/配置云助手客户端/安装云助手客户端.md)和[部署并使用Docker（Alibaba Cloud Linux 2）](/cn.zh-CN/建站教程/搭建应用/部署并使用Docker/部署并使用Docker（Alibaba Cloud Linux 2）.md)。


## 操作步骤

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在**新建采集器**区域中，单击**Auditbeat**。

3.  安装并配置采集器。

    详情请参见[采集ECS服务日志](/cn.zh-CN/Beats/采集ECS服务日志.md)和[采集器YML配置](/cn.zh-CN/Beats/采集器YML配置.md)，本文使用的配置如下。

    ![配置Auditbeat采集器](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1112659951/p86994.png)

    **说明：**

    -   勾选**启用Monitoring**，系统会在Kibana控制台开启Auditbeat服务的监控。
    -   勾选**启用Kibana Dashbord**，系统会在Kibana控制台中生成图表，无需额外配置Yml。由于阿里云Kibana配置在VPC内，因此需要先在Kibana配置页面开通Kibana私网访问功能，详情请参见[配置Kibana公网或私网访问白名单](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/配置Kibana公网或私网访问白名单.md)。
    本文使用默认的**auditbeat.yml**配置文件，无需更改。相关模块配置说明如下：

    -   Auditd模块配置

        ```
        - module: auditd
          # Load audit rules from separate files. Same format as audit.rules(7).
          audit_rule_files: [ '${path.config}/audit.rules.d/*.conf' ]
          audit_rules: |
        ```

        -   `audit_rule_files`：指定加载的审计规则文件，支持通配符，默认提供了32位和64位两种，请根据您的系统选择其一。
        -   `audit_rules`：定义审计规则。可连接ECS，执行`./auditbeat show auditd-rules`命令，查看默认的审计规则。

            ```
            -a never,exit -S all -F pid=26253
            -a always,exit -F arch=b32 -S all -F key=32bit-abi
            -a always,exit -F arch=b64 -S execve,execveat -F key=exec
            -a always,exit -F arch=b64 -S connect,accept,bind -F key=external-access
            -w /etc/group -p wa -k identity
            -w /etc/passwd -p wa -k identity
            -w /etc/gshadow -p wa -k identity
            -a always,exit -F arch=b64 -S open,truncate,ftruncate,create,openat,open_by_handle_at -F exit=-EACCES -F key=access
            -a always,exit -F arch=b64 -S open,truncate,ftruncate,create,openat,open_by_handle_at -F exit=-EPERM -F key=access
            ```

            **说明：** 一般情况下默认规则就可以满足审计需求。如果需要自定义审计规则，可修改audit.rules.d目录下的审计规则文件。

    -   File Integrity模块配置

        ```
        - module: file_integrity
          paths:
          - /bin
          - /usr/bin
          - /sbin
          - /usr/sbin
          - /etc
        ```

        `paths`：指定被监控文件的路径，默认监控的路径包括`/bin`、`/user/bin`、`/sbin`、`/usr/sbin`、`/etc`。

4.  选择采集器安装的ECS实例。

    ![选择采集器安装的实例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3112659951/p82419.png)

5.  启动并查看采集器安装情况。

    1.  单击**启动**。

        启动成功后，系统弹出**启动成功**对话框。

    2.  单击**前往采集中心查看**，返回**Beats数据采集中心**页面，在**采集器管理**区域中，查看启动成功的Auditbeat采集器。

    3.  等待**采集器状态**变为**已生效1/1**后，单击右侧**操作**栏下的**查看运行实例**。

    4.  在**查看运行实例**页面，查看**采集器安装情况**，当显示为**心跳正常**时，说明采集器安装成功。

        ![Auditbeat采集器安装成功](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1112659951/p87000.png)


## 查看结果

1.  登录目标阿里云ES实例的Kibana控制台。

    登录控制台的具体步骤请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Discover**，选择预定义的**auditbeat-\***模式，并选择一个时间段，查看对应时间段内的Auditbeat收集的数据。

    ![查看Auditbeat数据收集结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1112659951/p87004.png)

3.  在左侧导航栏，单击**Dashboard**。

4.  在**Dashboard**列表中，单击**\[Auditbeat File Integrity\] Overview**，然后选择一个时间段，查看该时间段内监控文件的更改情况。

    ![Auditbeat监控文件更改情况](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1112659951/p87008.png)


