---
keyword: 登录Kibana
---

# 登录Kibana控制台

当您购买了阿里云Elasticsearch实例后，我们会为您赠送一个1核2GB的Kibana节点，同时支持购买更高规格的Kibana节点。通过Kibana，您可以完成数据查询、数据可视化等操作。本文介绍如何登录Kibana控制台。

您已完成以下操作：

-   创建阿里云Elasticsearch实例。

    具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

-   开启Kibana公网或私网访问（默认开启）。

    具体操作，请参见[配置Kibana公网或私网访问白名单](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/配置Kibana公网或私网访问白名单.md)。

-   （可选）设置Kibana的语言模式。默认为英文，支持切换为中文。

    具体操作，请参见[配置Kibana语言](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/配置Kibana语言.md)。


阿里云Elasticsearch提供的Kibana控制台，为您的业务提供扩展的可能性。Kibana控制台作为Elastic生态系统的组成部分，支持无缝衔接Elasticsearch服务，可以让您实时了解服务的运行状态并进行管理。

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  在顶部菜单栏处，选择资源组和地域，然后在**实例列表**中单击目标实例ID。

4.  单击左侧导航栏的**可视化控制**。

5.  在**Kibana**区域中，单击**私网入口**或**公网入口**。

    -   **私网入口**：开启**Kibana私网访问**后（默认未开启），才会显示。具体操作，请参见[配置Kibana公网或私网访问白名单](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/配置Kibana公网或私网访问白名单.md)。
    -   **公网入口**：开启**Kibana公网访问**后（默认开启），才会显示。具体操作，请参见[配置Kibana公网或私网访问白名单](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/配置Kibana公网或私网访问白名单.md)。

        **说明：** 首次从**公网入口**进入Kibana控制台且公网访问配置未修改时，系统会提示您修改配置。单击**修改配置**，进入**Kibana配置**页面修改。修改后，再次单击**公网入口**，即可进入Kibana控制台。

6.  在Kibana登录页面，输入用户名和密码，单击登录。

    -   用户名：默认为elastic。您也可以创建自定义用户，具体操作请参见[创建用户](/cn.zh-CN/访问控制/Kibana角色管理/创建用户.md)。
    -   密码：对应用户的密码。elastic用户的密码在创建实例时设定，如果忘记可重置。重置密码的注意事项和操作步骤，请参见[重置实例访问密码](/cn.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。

登录成功后，您可以在Kibana控制台上完成数据查询、制作数据展示仪表板等操作。详细信息，请参见[Kibana Guide](https://www.elastic.co/guide/en/kibana/current/index.html)。

