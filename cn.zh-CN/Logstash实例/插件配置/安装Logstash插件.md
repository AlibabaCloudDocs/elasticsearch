---
keyword: 安装logstash插件
---

# 安装Logstash插件

在使用插件前，您必须先安装插件。本文介绍安装阿里云Logstash插件的方法。

已创建阿里云Logstash实例。具体操作，请参见[创建阿里云Logstash实例](/cn.zh-CN/Logstash实例/快速入门/步骤一：创建实例/创建阿里云Logstash实例.md)。

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Logstash实例**。

3.  在顶部菜单栏处，选择地域，然后在**实例列表**中单击目标实例ID。

4.  在左侧导航栏，单击**插件配置**。

5.  在**系统默认插件列表**中，单击目标插件右侧的**安装**。

    ![安装Logstash插件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4529919951/p95673.png)

    **警告：** 安装插件会触发实例重启，请确认后再执行以下步骤。

6.  在**安装提示**对话框中，阅读系统提示，单击**确认**。

    确认后，实例会重启。重启时，可在任务列表中[查看任务进度](/cn.zh-CN/Logstash实例/实例管理/查看实例任务进度详情.md)。重启成功后，即可完成插件的安装。

    **说明：** 插件安装成功后，如果不再使用，可单击对应插件右侧的**卸载**，使用同样的方式进行卸载。卸载插件也会触发集群重启，请确认后操作。


