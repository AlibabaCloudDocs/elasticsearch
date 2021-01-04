# 通过Uptime实时监控云Elasticsearch服务

Heartbeat支持通过HTTP/HTTPS、TCP和ICMP服务，定期检测网络端点状态，并将采集的检测数据，输出到Kibana的Uptime应用中，实时监控应用程序及服务的可用性和响应时间，在业务受到影响前检测出问题。本文介绍如何通过Uptime实时监控云Elasticsearch服务。

Uptime需要与以下服务结合使用：

-   Heartbeat
-   Elasticsearch
-   Kibana

**说明：** 您还可以通过Kibana 7.7的[Alerting and Actions](https://www.elastic.co/guide/en/kibana/7.7/alerting-getting-started.html#alerting-setup-prerequisites)实现监控报警通知。

## 部署架构

-   单实例部署

    单个Heartbeat实例部署在单个监控位置，监控单个服务。Heartbeat发送监控数据给Elasticsearch，与此同时，可以使用Kibana Uptime查看心跳数据并确定服务状态。

    ![单实例部署](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1271239061/p207825.png)

-   多实例部署

    两个Heartbeat部署在不同的监控位置，监控同一个服务。Heartbeat发送监控数据给Elasticsearch，与此同时，可以使用Kibana Uptime查看心跳数据并确定服务状态。当某个区域的Heartbeat发生故障，多个监视位置可以帮助您定位Heartbeat故障的区域。

    ![多实例部署](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1271239061/p207826.png)


更多部署架构，请参见[Deployment Architecture](https://www.elastic.co/guide/en/uptime/7.9/uptime-deployment-arch.html)。

## 准备工作

1.  创建阿里云Elasticsearch实例，并开启自动创建索引功能。

    具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)和[配置YML参数](/cn.zh-CN/Elasticsearch/集群配置/配置YML参数.md)。

2.  创建ECS实例，作为Heartbeat的部署机器。要求该ECS实例与阿里云Elasticsearch实例处于同一专有网络下。

    具体操作，请参见[使用向导创建实例](/cn.zh-CN/实例/创建实例/使用向导创建实例.md)。

    **说明：** Beats目前仅支持Aliyun Linux、RedHat和CentOS这三种操作系统。

3.  在ECS实例上安装云助手和Docker服务。

    具体操作，请参见[安装云助手客户端](/cn.zh-CN/运维与监控/云助手/配置云助手客户端/安装云助手客户端.md)和[部署并使用Docker（Alibaba Cloud Linux 2）](/cn.zh-CN/建站教程/搭建应用/部署并使用Docker/部署并使用Docker（Alibaba Cloud Linux 2）.md)。


## 创建Heartbeat采集器

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)，在左侧导航栏，单击**Beats数据采集中心**。

2.  在**创建采集器**区域，单击**Heartbeat**。

3.  安装并配置采集器。

    具体操作，请参见[安装采集器](/cn.zh-CN/Beats/安装采集器.md)和[采集器YML配置](/cn.zh-CN/Beats/采集器YML配置.md)。

    ![配置Heartbeat](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1271239061/p207864.png)

    |参数|说明|
    |--|--|
    |type|本文指定为http。**说明：** Heartbeat支持检查HTTP/HTTPS、TCP和ICMP服务。例如使用HTTP/HTTPS监视器，可以检查响应代码（code）、正文（body）和头信息（header）； 使用TCP监视器，可以检查端口和字符串。 |
    |urls|待检查的URL列表，可以指定多个HTTP服务。本文以检查阿里云Elasticsearch服务为例，此处需要配置为待检查实例的私网访问地址。|
    |schedule|检查间隔。@every 10s表示每10s检查一次。|

4.  单击**下一步**。

5.  在**采集器安装**配置向导中，选择安装采集器的ECS实例。

    ![选择采集器安装的实例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3112659951/p82419.png)

6.  启动采集器并查看采集器安装情况。

    具体操作，请参见[安装采集器](/cn.zh-CN/Beats/安装采集器.md)。

    当**采集器状态**为**已生效**，且**采集器安装情况**显示为**心跳正常**时，说明采集器安装成功。

    ![采集器安装成功](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1271239061/p208030.png)


## 查看Uptime监控信息

1.  登录Kibana控制台。

    此Kibana控制台为：创建采集器时，**采集器Output**指定的Elasticsearch实例对应的Kibana控制台。具体操作，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Uptime**，查看监控数据。

    ![查看Uptime监控信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1271239061/p208049.png)

    -   红色：异常状态，请检查Heartbeat通信或Elasticsearch状态。
    -   蓝色：正常状态。

