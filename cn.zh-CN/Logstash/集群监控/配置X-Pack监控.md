# 配置X-Pack监控

本文介绍如何通过配置X-Pack来监控阿里云Logstash服务。开启X-Pack监控，并关联阿里云Elasticsearch实例后，即可在Kibana中监控Logstash服务。

您已完成以下操作：

-   创建阿里云Logstash实例。

    具体操作，请参见[创建阿里云Logstash实例](/cn.zh-CN/Logstash/快速入门/步骤一：创建实例/创建阿里云Logstash实例.md)。

-   创建阿里云Elasticsearch实例，要求与Logstash实例在同一专有网络下，且大版本相同。

    具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

-   您已开通目标Elasticsearch实例的Kibana公网访问。

    具体操作，请参见[配置Kibana公网或私网访问白名单](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/配置Kibana公网或私网访问白名单.md)。


1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在顶部菜单栏处，选择地域。

3.  在左侧导航栏，单击**Logstash实例**，然后在**Logstash实例**中单击目标实例ID。

4.  在左侧导航栏，单击**集群监控**。

5.  在**监控报警配置**区域，单击**X-Pack监控**右侧的**修改**。

    ![开启X-Pack监控](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2629919951/p67546.png)

6.  在**修改配置**页面，开启**X-Pack监控**，并配置要关联的阿里云Elasticsearch实例。

    ![修改配置页面](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2629919951/p67560.png)

    |参数|说明|
    |--|--|
    |**Elasticsearch实例**|选择需要关联的阿里云Elasticsearch实例，要求与Logstash实例在同一专有网络下，且大版本相同（如果版本不相同，请务必保证版本间的兼容性）。|
    |**用户名**|访问阿里云Elasticsearch实例的用户名。|
    |**密码**|访问阿里云Elasticsearch实例的密码。|

7.  单击**测试连通性**。

    无报错即连通成功。

    **警告：** 修改X-Pack配置会触发实例重启，请在不影响业务的情况下，继续执行以下步骤。

8.  单击**确定**。

    确定后，系统返回**集群监控**页面，并触发实例重启。

9.  等待重启完成后，查看Logstash监控信息。

    重启完成后，**X-Pack监控**显示为**开启**，且在当前页面显示所关联的阿里云Elasticsearch实例。

    **说明：** 重启完成后，才可在Kibana控制台上查看到Logstash监控信息。

    1.  在**集群监控**页面，单击**前往Kibana控制台管理**。

        ![X-Pack监控开启结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2629919951/p67571.png)

    2.  登录Kibana控制台。

        具体操作，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

    3.  在左侧导航栏，单击**Monitoring**。

    4.  在**Logstash**区域，查看Logstash的监控信息。

        ![Logstash监控页面](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2629919951/p67575.png)


