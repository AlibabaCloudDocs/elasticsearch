---
keyword: [Metricbeat采集器, Metricbeat收集系统数据, Metricbeat收集Nginx服务数据]
---

# 通过Metricbeat收集系统数据及Nginx服务数据

本文介绍如何通过阿里云Metricbeat采集器收集系统数据（CPU使用率、内存、磁盘IO和网络IO统计数据）和Nginx服务数据，并生成可视化图表。

您已完成以下操作：

-   创建阿里云Elasticsearch（简称ES）实例。

    详情请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

-   开启阿里云ES实例的**自动创建索引**功能。

    处于安全考虑，阿里云ES默认不允许**自动创建索引**。但是Beats目前依赖该功能，因此如果**采集器Output**选择为**Elasticsearch**，需要开启**自动创建索引**功能，详情请参见[开启自动创建索引](/cn.zh-CN/Elasticsearch/快速访问与配置.md)。

-   创建阿里云ECS实例，且该ECS实例与阿里云ES实例处于同一专有网络VPC（Virtual Private Cloud）下。

    详情请参见[使用向导创建实例](/cn.zh-CN/实例/创建实例/使用向导创建实例.md)。

    **说明：** Beats目前仅支持Aliyun Linux、RedHat和CentOS这三种操作系统。

-   在目标ECS实例上安装云助手和Docker服务。

    详情请参见[安装云助手客户端](/cn.zh-CN/运维与监控/云助手/配置云助手客户端/安装云助手客户端.md)和[部署并使用Docker（Alibaba Cloud Linux 2）](/cn.zh-CN/建站教程/搭建应用/部署并使用Docker/部署并使用Docker（Alibaba Cloud Linux 2）.md)。


## 使用Metricbeat收集系统数据

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)[阿里云Elasticsearch控制台](https://partners-intl.elasticsearch.console.aliyun.com/#/home)。

2.  在**新建采集器**区域中，单击**Metricbeat**。

3.  安装并配置采集器。

    详情请参见[采集ECS服务日志](/cn.zh-CN/Beats/采集ECS服务日志.md)和[采集器YML配置](/cn.zh-CN/Beats/采集器YML配置.md)，本文使用的配置如下。

    ![Metricbeat](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9012659951/p86406.png)

    **说明：**

    -   勾选**启用Monitoring**，系统会在Kibana控制台开启Metricbeat服务的监控。
    -   勾选**启用Kibana Dashbord**，系统会在Kibana控制台中生成图表，无需额外配置Yml。由于阿里云Kibana配置在VPC内，因此需要先在Kibana配置页面开通Kibana私网访问功能，详情请参见[配置Kibana公网或私网访问白名单](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/配置Kibana公网或私网访问白名单.md)。
    -   由于系统默认开启了system模块，因此无需进行**采集器Yml配置**。
4.  单击**下一步**。

5.  选择采集器安装的ECS实例。

    ![选择采集器安装的实例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3112659951/p82419.png)

    **说明：** 如果您是初次创建采集器，请先单击**前往授权**，按照提示为阿里云ES授予访问阿里云ECS的权限。

6.  启动采集器并查看采集器安装情况。

    1.  单击**启动**。

        启动成功后，系统弹出**启动成功**对话框。

    2.  单击**前往采集中心查看**，返回**Beats数据采集中心**页面，在**采集器管理**区域中，查看启动成功的Metricbeat采集器。

    3.  等待**采集器状态**变为**已生效1/1**后，单击右侧**操作**栏下的**查看运行实例**。

    4.  在**查看运行实例**页面，查看**采集器安装情况**，当显示为**心跳正常**时，说明采集器安装成功。

        ![采集器安装成功](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9012659951/p86408.png)

7.  查看结果。

    1.  登录目标阿里云ES实例的Kibana控制台。

        登录控制台的具体步骤请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

    2.  在左侧导航栏，单击**Dashboard**。

    3.  在**Dashboard**列表中，单击**\[Metricbeat System\] Overview**，再单击对应的Metricbeat系统，查看该系统的监控仪表板。

        ![仪表板](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9012659951/p86416.png)


## 使用Metricbeat收集Nginx服务数据

前提条件：开启Nginx服务的`stub_status`。由于`ngx_http_stub_status_module`模块是Nginx中用来统计Nginx服务所接收和处理的请求数量，因此需要在nginx.conf文件中启用`stub_status`。

```
location /status {
           stub_status on;
           access_log off;
        }
```

**说明：** 下文中metricbeat.yml文件中配置的`server_status_path`要与nginx.conf中的`status`保持一致。

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)[阿里云Elasticsearch控制台](https://partners-intl.elasticsearch.console.aliyun.com/#/home)。

2.  在**新建采集器**区域中，单击**Metricbeat**。

3.  安装并配置采集器。

    详情请参见[采集ECS服务日志](/cn.zh-CN/Beats/采集ECS服务日志.md)和[采集器YML配置](/cn.zh-CN/Beats/采集器YML配置.md)，本文使用的配置如下。

    ![监控nginx性能采集器配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9012659951/p86418.png)

    需要在**metricbeat.yml**中添加如下脚本。

    ```
    metricbeat.modules:
    - module: nginx
      metricsets: ["stubstatus"]
      enabled: true
      period: 10s
      # Nginx hosts
      hosts: ["http://121.41.**.**"]
      # Path to server status. Default server-status
      server_status_path: "status"
    ```

    **说明：**

    -   勾选**启用Monitoring**，系统会在Kibana控制台开启Metricbeat服务的监控。
    -   勾选**启用Kibana Dashbord**，系统会在Kibana控制台中生成图表，无需额外配置Yml。由于阿里云Kibana配置在VPC内，因此需要先在Kibana配置页面开通Kibana私网访问功能，详情请参见[配置Kibana公网或私网访问白名单](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/配置Kibana公网或私网访问白名单.md)。
4.  单击**下一步**。

5.  选择采集器安装的ECS实例。

    ![选择采集器安装的实例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3112659951/p82419.png)

    **说明：** 如果您是初次创建采集器，请先单击**前往授权**，按照提示为阿里云ES授予访问阿里云ECS的权限。

6.  启动并查看采集器安装情况。

    详细操作方法请参见[使用Metricbeat收集系统数据](#section_3rx_xw8_rbi)。

7.  查看结果。

    1.  在浏览器中，访问`<Nginx hosts>/status`，打开监控页面。

        ![访问监控页面](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9012659951/p86420.png)

    2.  登录目标阿里云ES实例的Kibana控制台。

        登录控制台的具体步骤请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

    3.  在左侧导航栏，单击**Dashboard**。

    4.  在**Dashboard**列表中，单击**\[Metricbeat Nginx\] Overview**，查看Nginx服务的监控仪表板。

        ![nginx监控仪表板](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9012659951/p86423.png)


