---
keyword: cerebro访问es
---

# 通过Cerebro访问阿里云ES

除了Kibana、curl命令、客户端等方式，您还可以通过Elasticsearch-Head、Cerebro等第三方插件或工具访问阿里云Elasticsearch（简称ES）实例。由于Elasticsearch-Head插件在5.x版本之后已不再维护，因此建议您使用Cerebro访问阿里云ES实例。本文介绍具体的操作方法。

-   创建阿里云ES实例。

    具体操作步骤请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

-   创建ECS实例，要求该实例与阿里云ES实例在同一专有网络VPC（Virtual Private Cloud）下。

    具体操作步骤请参见[使用向导创建实例](/intl.zh-CN/实例/创建实例/使用向导创建实例.md)。该ECS实例用来安装Cerebro。

    **说明：** 如果您的ECS实例与阿里云ES实例不在同一VPC中，或者您需要在本机安装cerebro，此时可通过公网访问阿里云ES实例。通过公网访问阿里云ES时需要注意：

    -   公网访问的安全性较低。
    -   当网络延迟时可能会造成服务抖动。
    -   需要开启阿里云ES的公网地址并配置公网访问白名单，详情请参见[配置ES公网或私网访问白名单](/intl.zh-CN/实例管理/安全配置/配置ES公网或私网访问白名单.md)。
-   在ECS实例中安装[JDK](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)，要求版本为1.8及以上。

-   [Cerebro](https://docs.cerebrohq.com/en/articles/3292944-what-is-cerebro)是第三方支持的工具。
-   在公网环境下，Cerebro只能通过阿里云ES实例的公网地址和端口访问集群。

1.  连接ECS实例。

    具体操作步骤请参见[连接ECS实例]()。

2.  下载[Cerebro](https://github.com/lmenezes/cerebro/releases)安装包并解压。

    -   下载

        ```
        wget https://github.com/lmenezes/cerebro/releases/download/v0.9.0/cerebro-0.9.0.tgz
        ```

    -   解压

        ```
        tar -zxvf cerebro-0.9.0.tgz
        ```

3.  修改Cerebro配置文件，关联待访问的阿里云ES实例。

    1.  打开application.conf文件。

        ```
        vim cerebro-0.9.0/conf/application.conf
        ```

    2.  按照以下说明配置`hosts`。

        ![配置cerebro](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0312659951/p128887.png)

        **说明：** 您也可以关联多个实例，多个实例之间用英文逗号（,）分隔。

        |参数|说明|
        |--|--|
        |host|阿里云ES实例的访问地址，格式为`http://<阿里云ES实例的内网地址>:9200`。实例的内网地址可在基本信息页面获取，详情请参见[查看实例的基本信息](/intl.zh-CN/实例管理/管理实例/查看实例的基本信息.md)。|
        |name|阿里云ES实例的ID，可在基本信息页面获取，详情请参见[查看实例的基本信息](/intl.zh-CN/实例管理/管理实例/查看实例的基本信息.md)。|
        |username|访问阿里云ES实例的用户名，默认为elastic。 **说明：** 实际业务中不建议使用elastic用户，这样会降低系统安全性。建议使用自建用户，并给予自建用户分配相应的角色和权限，详情请参见[创建角色](/intl.zh-CN/访问控制/Kibana角色管理/创建角色.md)和[创建用户](/intl.zh-CN/访问控制/Kibana角色管理/创建用户.md)。 |
        |password|对应用户的密码。elastic用户的密码在创建实例时指定，如果忘记可进行重置，重置密码的注意事项和操作步骤请参见[重置实例访问密码](/intl.zh-CN/实例管理/安全配置/重置实例访问密码.md)。|

    3.  保存文件后，启动Cerebro服务。

        ```
        cd cerebro-0.9.0
        bin/cerebro
        ```

        启动成功后，返回如下结果。

        ![cerebro启动成功](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0312659951/p128924.png)

4.  通过Cerebro访问阿里云ES。

    1.  配置ECS实例的安全组，在**入方向**中，添加待访问机器的IP地址并开放9000端口。

        具体操作步骤请参见[添加安全组规则](/intl.zh-CN/安全/安全组/添加安全组规则.md)。

        ![配置安全组](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0312659951/p128925.png)

    2.  在浏览器中输入http://<ECS的外网IP地址\>:9000。

    3.  在Cerebro登录页面，单击您要访问的阿里云ES实例的ID。

        ![单击集群名称](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0312659951/p128926.png)

    4.  在Cerebro控制台中，查看集群状态以及索引、分片和文档数量等，并根据业务进行相关操作。

        ![cerebro控制台](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0312659951/p128927.png)

        **说明：** Cerebro的使用方法请参见[Getting Started with Cerebro](https://docs.cerebrohq.com/en/collections/1978931-getting-started-with-cerebro)。


