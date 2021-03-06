---
keyword: [配置es白名单, 配置es公网白名单, 配置es私网白名单]
---

# 配置Elasticsearch公网或私网访问白名单

当您需要通过公网或私网来访问阿里云Elasticsearch实例时，可将待访问设备的IP地址加入到实例的公网或私网访问白名单中。本文介绍如何配置Elasticsearch实例的公网或私网访问白名单。

已创建阿里云Elasticsearch实例。具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  在顶部菜单栏处，选择资源组和地域，然后在**实例列表**中单击目标实例ID。

4.  在左侧导航栏，单击**安全配置**。

5.  在**集群网络设置**区域，打开**公网地址**开关（默认关闭），开启公网访问。

    **说明：** 如果已经开启了公网访问，或仅需要配置VPC私网访问白名单，可忽略此步骤。

    开启后开关显示为**绿色**，默认显示为灰色，即关闭状态。公网地址开启后，才可使用公网地址访问实例。

6.  单击**修改**，在白名单输入框中输入需要添加的IP地址。

    ![配置ES的公网或私网访问白名单](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5946359951/p95980.png)

    公网和私网访问白名单都支持配置为单个IP地址或IP网段的形式，格式为`192.168.0.1`或`192.168.0.0/24`，多个IP地址之间用英文逗号隔开。`127.0.0.1`代表禁止所有IPv4地址访问，`0.0.0.0/0`代表允许所有IPv4地址访问。并且白名单下的IP地址或者IP网段数量最多支持300个。两者区别如下。

    |类别|说明|
    |--|--|
    |公网地址访问白名单|    -   目前杭州区域支持公网IPv6地址访问，并可以配置IPv6地址白名单，格式为`2401:b180:1000:24::5`或`2401:b180:1000::/48`。`::1`代表禁止所有IPv6地址访问，`::/0`代表允许所有IPv6地址访问。
    -   默认禁止所有公网地址访问。 |
    |VPC私网访问白名单|默认允许所有私网IPv4地址访问。|

7.  单击**确定**。


