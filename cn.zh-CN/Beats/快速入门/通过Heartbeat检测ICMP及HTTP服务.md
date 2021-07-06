---
keyword: [Heartbeat检测ICMP服务, Heartbeat检测HTTP服务]
---

# 通过Heartbeat检测ICMP及HTTP服务

本文介绍如何通过阿里云Heartbeat检测ICMP及HTTP服务的状态，并生成可视化图表。

Heartbeat是一个轻量级的守护程序，可以安装在远程服务器上，定期检测服务状态并确定它们是否可用。与Metricbeat不同，Metricbeat仅检测服务器是启动还是关闭，Heartbeat会检测服务是否可以访问。

**说明：** 与大多数需要安装在边缘节点的Beats不同，Heartbeat可以安装为在单独的计算机上，甚至可以处于监视服务运行的网络之外。

目前Heartbeat支持以下三种监视器：

-   ICMP监视器（包括ICMPV4和ICMPV6）：使用ICMP协议连接服务，通过发送ICMP请求，检测服务是否可用（需要ROOT权限）。
-   TCP监视器：使用TCP协议连接服务，通过发送或者接受特定的负载，检测服务是否可用及服务状态是否正常。
-   HTTP监视器：使用HTTP协议连接服务，通过特定的状态码、响应头或者内容，检测服务是否可用及服务状态是否正常。

    **说明：** TCP和HTTP监视器都支持SSL、TLS以及部分代理设置。


## 前提条件

-   创建阿里云Elasticsearch（简称ES）实例。

    详情请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

-   开启阿里云ES实例的**自动创建索引**功能。

    出于安全考虑，阿里云ES默认不允许**自动创建索引**。但是Beats目前依赖该功能，因此如果**采集器Output**选择为**Elasticsearch**，需要开启**自动创建索引**功能，详情请参见[开启自动创建索引](/cn.zh-CN/Elasticsearch/快速访问与配置.md)。

-   创建阿里云ECS实例，且该ECS实例与阿里云ES实例处于同一专有网络VPC（Virtual Private Cloud）下。

    详情请参见[使用向导创建实例](/cn.zh-CN/实例/创建实例/使用向导创建实例.md)。

    **说明：** Beats目前仅支持Aliyun Linux、RedHat和CentOS这三种操作系统。

-   在目标ECS实例上安装云助手和Docker服务。

    详情请参见[安装云助手客户端](/cn.zh-CN/运维与监控/云助手/配置云助手客户端/安装云助手客户端.md)和[部署并使用Docker（Alibaba Cloud Linux 2）](/cn.zh-CN/建站教程/搭建应用/部署并使用Docker/部署并使用Docker（Alibaba Cloud Linux 2）.md)。


## 操作步骤

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)[阿里云Elasticsearch控制台](https://partners-intl.elasticsearch.console.aliyun.com/#/home)。

2.  在**新建采集器**区域中，单击**Heartbeat**。

3.  安装并配置采集器。

    详情请参见[采集ECS服务日志](/cn.zh-CN/Beats/采集ECS服务日志.md)和[采集器YML配置](/cn.zh-CN/Beats/采集器YML配置.md)，本文使用的配置如下。

    ![heartbeat配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3112659951/p88191.png)

    **说明：**

    -   勾选**启用Monitoring**，系统会在Kibana控制台开启Heartbeat服务的监控。
    -   勾选**启用Kibana Dashbord**，系统会在Kibana控制台中生成图表，无需额外配置Yml。由于阿里云Kibana配置在VPC内，因此需要先在Kibana配置页面开通Kibana私网访问功能，详情请参见[配置Kibana公网或私网访问白名单](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/配置Kibana公网或私网访问白名单.md)。
    配置采集器时，需要在**heartbeat.yml**中配置`heartbeat.monitors`参数设置监视器，本案例使用的配置如下。

    ```
    heartbeat.monitors:
    - type: icmp
      schedule: '*/5 * * * * * *'
      hosts: ["47.111.xx.xx"]
    - type: http
      # List or urls to query
      urls: ["https://es-cn-xxxxx.kibana.elasticsearch.aliyuncs.com:5601/"]
      # Configure task schedule
      schedule: '@every 10s'
      check.response.status: 200
    ```

    |参数|说明|
    |--|--|
    |`type`|指定监视器的类型。本文分别指定`icmp`和`http`监视器。|
    |`schedule`|指定任务计划。`schedule: '*/5 * * * * * *'`表示每5秒执行一次任务；`schedule: '@every 10s'`表示从启动Heartbeat任务起，每10秒执行1次任务。|
    |`hosts`|指定要检测的主机列表。|
    |`urls`|指定要检测的URL列表。|
    |`check.response.status`|指定要检测的HTTP请求状态。`check.response.status: 200`表示如果请求的返回结果是`200`，那么服务是正常的。|

    **说明：** 更多参数说明请参见[官方Heartbeat配置文档](https://www.elastic.co/guide/en/beats/heartbeat/6.8/configuration-heartbeat-options.html)。

4.  选择采集器安装的ECS实例。

    ![选择采集器安装的实例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3112659951/p82419.png)

    所选择的ECS实例，需要满足上文的前提条件。

5.  启动并查看采集器安装情况。

    1.  单击**启动**。

        启动成功后，系统弹出**启动成功**对话框。

    2.  单击**前往采集中心查看**，返回**Beats数据采集中心**页面，在**采集器管理**区域中，查看启动成功的Heartbeat采集器。

    3.  等待**采集器状态**变为**已生效1/1**后，单击右侧**操作**栏下的**查看运行实例**。

    4.  在**查看运行实例**页面，查看**采集器安装情况**，当显示为**心跳正常**时，说明采集器安装成功。

        ![查看采集器安装状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3112659951/p82426.png)


## 查看结果

1.  登录目标阿里云ES实例的Kibana控制台。

    登录控制台的具体步骤请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Discover**，选择预定义的**heartbeat-\***模式，并选择一个时间段，查看对应时间段内Heartbeat收集的数据。

    ![Heartbeat收集的数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3112659951/p88202.png)

3.  在左侧导航栏，单击**Dashboard**。

4.  在**Dashboard**列表中，单击**Heartbeat HTTP monitoring**，然后选择一个时间段，查看该时间段内HTTP的状态统计图。

    ![HTTP监控状态图](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3112659951/p88201.png)


