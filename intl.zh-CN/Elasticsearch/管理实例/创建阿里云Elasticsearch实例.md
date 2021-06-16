---
keyword: 创建Elasticsearch实例
---

# 创建阿里云Elasticsearch实例

本文介绍如何创建阿里云Elasticsearch实例。

您已完成以下操作：

-   注册阿里云账号。

    具体操作，请参见[创建阿里云账号](https://account.aliyun.com/register/register.html)。

-   开通专有网络和虚拟交换机服务。

    具体操作，请参见[搭建IPv4专有网络](/intl.zh-CN/快速入门/搭建IPv4专有网络.md)。

-   完成规格容量评估。

    具体操作，请参见[规格容量评估]()。


## 操作步骤

1.  前往[实例创建页面](https://common-buy-intl.alibabacloud.com/?spm=a2796.11423565.3381327530.3.6a9a32c2emlY47&commodityCode=elasticsearch_intl#/buy)。

2.  在购买页面，完成实例启动配置。

    配置参数的详细说明，请参见[购买页面参数](/intl.zh-CN/Elasticsearch/快速入门/步骤一：创建实例/购买页面参数.md)。

    **说明：**

    -   在前期程序研发或功能测试期间，建议购买按量付费实例测试。
    -   购买包年包月实例可以享受优惠条件。
3.  单击**立即购买**，进入**确认订单**页面查看实例配置。无误后，勾选服务协议，单击**立即开通**。

4.  提示开通成功后，进入[Elasticsearch概览](https://elasticsearchnext.console.aliyun.com/)页面。

5.  在左侧导航栏，单击**Elasticsearch实例**。在顶部菜单栏，选择资源组和地域，然后在**实例列表**页面查看创建成功的阿里云Elasticsearch实例。

6.  单击实例ID，在**基本信息**页面，单击右上角的![查看任务进度详细图标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0461458061/p203761.png)图标，查看实例的创建进度。

    **说明：**

    -   实例创建后，需要一段时间才能生效。时间长短与您的集群规格、数据结构和大小等相关，一般在小时级别。
    -   当实例的信息没有及时更新时，例如刚创建完成的实例状态显示失败，可在**基本信息**页面，单击**刷新**，手动刷新页面中的状态信息。

## 相关文档

[createInstance](/intl.zh-CN/API参考/Elasticsearch/实例管理/createInstance.md)

