---
keyword: [BSearch-Label插件, 数据打标]
---

# 使用BSearch-Label插件为数据打标

BSearch-Label是一个纯前端的数据打标插件。通过BSearch-Label插件，您无需编写复杂的DSL语句，即可以可视化的方式完成数据打标。本文介绍如何使用BSearch-Label插件为数据打标。

您已完成以下操作：

-   创建阿里云Elasticsearch实例，且实例版本为6.3或6.7。

    具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。本文以6.3版本为例。

-   安装BSearch-Label插件。

    具体操作，请参见[安装Kibana插件](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/安装Kibana插件.md)。

-   准备待打标的索引数据。

    具体操作，请参见[快速开始](/cn.zh-CN/Elasticsearch/快速开始.md)。

-   （可选）设置Kibana的语言模式。默认为英文，支持切换为中文。

    具体操作，请参见[配置Kibana语言](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/配置Kibana语言.md)，本文使用中文。


通常情况下，在分析数据的时候，您可能不仅是单纯的浏览，而是希望通过某些查询条件对数据进行分析，并对某个字段（或者新增一个字段）赋予一个特殊的值（标签）来标注不同的数据，这一过程被称为“打标”。对数据打标后，您可以根据这个标签进行聚合分类统计，也可以根据标签的不同值进行快速过滤。标注的数据还可以直接为后续的流程所使用。

1.  登录对应阿里云Elasticsearch实例的Kibana控制台。

    具体操作，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Management**，按照以下步骤创建一个索引模式。

    **说明：** 如果已经创建了索引模式，可忽略此步骤。

    1.  在**Management**页面，单击**Kibana**区域中的**Index Patterns**。

    2.  在**Create index pattern**页面，输入索引模式名称（待查询的索引名称）。

    3.  单击**Next step**。

        ![创建索引模式](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7934659951/p94699.png)

    4.  单击**Create index pattern**。

3.  在左侧导航栏，单击**Discover**。

4.  在**Discover**页面，单击右上角菜单栏的**打标**。

5.  根据需求，选择以下任意一种方式完成数据打标。

    -   对已有字段进行打标，示例如下。

        ![对已有字段进行打标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9056359951/p55145.jpg)

        1.  查询名字是张三的数据。
        2.  选择**age**字段，将其标记为**18**。
        3.  单击**确认打标**。
        4.  打开**历史打标**开关，查看历史打标任务详情。

            ![历史打标结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9056359951/p55146.png)

    -   对新增字段进行打标，示例如下。

        ![新增字段进行打标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9056359951/p55149.jpg)

        1.  查询名字是张三的数据。
        2.  勾选**自定义打标字段**。
        3.  新增一个字段**tag**，并将其标记为**teenager**。
        4.  单击**确认打标**。
        5.  查看打标结果。

            ![查看打标结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0156359951/p55152.png)


