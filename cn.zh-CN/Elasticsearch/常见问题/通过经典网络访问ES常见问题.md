# 通过经典网络访问ES常见问题

本文介绍通过经典网络访问阿里云Elasticsearch（简称ES）的常见问题。

## 如何通过经典网络访问专有网络中的阿里云ES？

从网络安全角度考虑，阿里云ES部署在您自有的专有网络VPC（Virtual Private Cloud）中。如果您的业务系统处于经典网络中，可以通过专有网络VPC中提供的[Classiclink](/cn.zh-CN/Elasticsearch/常见问题/通过经典网络访问ES常见问题.md)功能，打通经典网络访问专有网络VPC的通道，实现从经典网络访问专有网络VPC内的阿里云ES。

## 什么是Classiclink？

Classiclink是阿里云专有网络VPC提供的，经典网络访问专有网络VPC的网络通道。

## ClassicLink连接的限制有哪些？

-   最多允许1000台经典网络ECS实例连接到同一个VPC。
-   同账号且同地域下，一台经典网络ECS实例只能连接到一个VPC。

    如果要进行跨账号连接，例如将账号A的ECS实例连接到账号B的VPC，可以将ECS实例从账号A过户到账号B。

    您可以[提交工单](https://selfservice.console.aliyun.com/ticket/category/vpc/today)申请ECS实例过户。过户前，确保您已了解[ECS实例过户须知](https://help.aliyun.com/document_detail/58064.html)。

-   经典网络ECS实例只支持与VPC主网段下的ECS实例通信，而不支持与VPC附加网段下的ECS实例通信。
-   VPC要开启ClassicLink功能，需要满足以下条件。

    |VPC网段|限制|
    |:----|:-|
    |172.16.0.0/12|该VPC中不存在目标网段为10.0.0.0/8的自定义路由条目。|
    |10.0.0.0/8|    -   该VPC中不存在目标网段为10.0.0.0/8的自定义路由条目。
    -   确保和经典网络ECS实例通信的交换机的网段在10.111.0.0/16内。 |
    |192.168.0.0/16|    -   该VPC中不存在目标网段为10.0.0.0/8的自定义路由条目。
    -   需要在经典网络ECS实例中增加192.168.0.0/16指向私网网卡的路由。您可以使用提供的脚本添加路由，下载[路由脚本](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/58095/cn_zh/1502878832385/route192.zip)。

**说明：** 在运行脚本前，请仔细阅读脚本中包含的readme。 |


## 如何开启ClassicLink功能？

1.  登录[专有网络管理控制台](https://vpcnext.console.aliyun.com/vpc/cn-hangzhou/vpcs)。
2.  在左侧导航栏，单击**专有网络**。
3.  在顶部菜单栏，选择地域。
4.  找到目标专有网络，单击实例ID。

    推荐您选择172.16.0.0/12网段的VPC。

5.  在专有网络详情页面，单击**开启ClassicLink**。

    **说明：** 如果已经开启，则显示为**关闭ClassicLink**。

6.  在**开启ClassicLink**对话框中，单击**确定**。

    开启ClassicLink后，ClassicLink的状态变更为**已开启**。


## 如何建立ClassicLink连接？

建立ClassicLink连接前，请先完成以下准备工作：

-   了解建立ClassicLink连接的限制，详情请参见[ClassicLink的连接限制](/cn.zh-CN/Elasticsearch/常见问题/通过经典网络访问ES常见问题.md)。
-   在要建立ClassicLink连接的专有网络中开启了ClassicLink功能，详情请参见[开启ClassicLink功能](/cn.zh-CN/Elasticsearch/常见问题/通过经典网络访问ES常见问题.md)。

1.  登录[ECS管理控制台](https://ecs.console.aliyun.com)。
2.  在左侧导航栏，选择**实例与镜像** \> **实例**。
3.  在顶部菜单栏，选择地域。
4.  找到目标**经典网络**类型的ECS实例，在**操作**列中，选择**更多** \> **网络和安全组** \> **设置专有网络连接状态**。
5.  在弹出的对话框中选择目标专有网络VPC，单击**确定**，然后单击**前往实例安全组列表添加classicLink安全组规则**。

    ![添加ClassicLink安全组规则](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4156359951/p54981.png)

6.  单击**添加ClassicLink安全组规则**，根据以下信息配置ClassicLink安全组规则，然后单击**确定**。

    |配置|说明|
    |:-|:-|
    |**经典网络安全组**|显示经典网络安全组的名称。|
    |**选择专有网络安全组**|选择专有网络的安全组。|
    |**授权方式**|选择一种授权方式：     -   （推荐）**经典网络 <=\> 专有网络**：相互授权访问。
    -   **经典网络 =\> 专有网络**：授权经典网络ECS访问专有网络内的云资源。
    -   **专有网络 =\> 经典网络**：授权专有网络内的云资源访问经典网络ECS。 |
    |**协议类型**|选择授权通信的协议，例如**自定义TCP**。|
    |**端口范围**|选择授权通信的端口。端口的输入格式为xx/xx，例如授权80端口，则输入80/80。|
    |**优先级**|设置该规则的优先级。数字越小，优先级越高。例如：1。|
    |**描述**|输入安全组描述。|


## 如何验证经典网络和VPC互通？

可以通过以下两种方式来验证经典网络和VPC是否互通：

-   （推荐）返回[ECS管理控制台](https://ecs.console.aliyun.com)，选择**实例与镜像** \> **实例**，单击右侧的![自定义列表项](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4682185161/p249853.png)图标，在弹出的对话框中勾选**连接状态**复选框，然后单击**确定**查看ECS实例的连接状态。
-   登录连接了Classiclink的ECS实例，通过curl命令访问对应VPC网络环境中的阿里云ES实例。

    **说明：** 如果系统提示curl command not found，请先使用`yum install curl`命令，在ECS中安装curl命令。

    ```
    curl -u <username>:<password> http://<host>:<port>
    ```

    |变量名|说明|
    |---|--|
    |`<username>`|阿里云ES实例的访问账号，建议通过非elastic账号访问。 **说明：**

    -   支持通过elastic账号访问，但因为在修改elastic账号对应密码后需要一些时间来生效，在密码生效期间会影响服务访问，因此不建议通过elastic来访问。
    -   如果您创建的阿里云ES实例版本包含with\_X-Pack信息，则访问该阿里云ES实例时，必须指定用户名和密码。 |
    |`<password>`|阿里云ES实例的密码，为您在创建ES实例时设置的密码，或初始化Kibana时指定的密码。|
    |`<host>`|阿里云ES实例的**私网地址**，可在实例的**基本信息**页面获取。|
    |`<port>`|阿里云ES实例的端口，一般为**9200**，可在实例的**基本信息**页面获取。|

    访问示例如下。

    ```
    curl -u elastic:es_password http://es-cn-vxxxxxxxxxxxxmedp.elasticsearch.aliyuncs.com:9200
    ```

    访问成功后，返回如下结果。

    ![使用ECS访问ES结果示例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1869559951/p58858.png)


