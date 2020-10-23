# 创建阿里云Logstash实例

本文介绍创建阿里云Logstash实例的方法。

您已经完成以下操作：

-   注册阿里云账号。

    详情请参见[账号注册](https://account.aliyun.com/register/register.html)。

-   开通专有网络VPC和虚拟交换机服务。

    详情请参见[创建专有网络和虚拟交换机](/cn.zh-CN/Logstash实例/快速入门/准备工作.md)。

    **说明：** 目前阿里云Logstash数据推送只支持同一专有网络VPC（Virtual Private Cloud），相同版本的阿里云Elasticsearch（简称ES）。


1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Logstash实例**。

3.  在**实例列表**页面，单击**创建**。

4.  在购买页面的前三个配置页面，完成实例启动配置。

    本教程选择实例的**付费模式**为**按量付费**，**Logstash版本**为**6.7**，其余配置均保持默认，更多配置信息详情请参见[购买页面参数](/cn.zh-CN/Logstash实例/快速入门/步骤一：创建实例/购买页面参数.md)。

    **说明：**

    -   在前期程序研发或功能测试期间，建议购买**按量付费**类型的阿里云Logstash实例进行测试。
    -   购买**包年包月**类型的阿里云Logstash可以享受优惠条件。
5.  单击**下一步：确认订单**，预览实例配置。

    配置不符合预期时，可单击![修改实例配置](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0546359951/p84860.png)图标进行修改。

    本教程的实例配置预览如下图。

    ![Logstash实例配置](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1618343061/p85383.png)

6.  勾选**阿里云LogstashService（按量付费）服务协议**，单击**立即购买**。

7.  提示开通成功后，单击**管理控制台**，进入Logstash的**实例列表**页面。


等待实例**状态**变为**正常**，即可开始[步骤二：创建并运行管道任务](/cn.zh-CN/Logstash实例/快速入门/步骤二：创建并运行管道任务.md)。

