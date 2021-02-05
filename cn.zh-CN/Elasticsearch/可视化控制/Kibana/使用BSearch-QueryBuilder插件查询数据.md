---
keyword: [BSearch-QueryBuilder插件, es高级查询]
---

# 使用BSearch-QueryBuilder插件查询数据

BSearch-QueryBuilder又称高级查询，是一个纯前端的工具插件。通过BSearch-QueryBuilder插件，您无需编写复杂的DSL语句，即可以可视化的方式完成复杂的查询请求。本文介绍如何使用BSearch-QueryBuilder插件查询数据。

您已完成以下操作：

-   创建阿里云Elasticsearch实例，且实例版本为6.3或6.7。

    具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。本文以6.3版本为例。

-   安装BSearch-QueryBuilder插件。

    具体操作，请参见[安装Kibana插件](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/安装Kibana插件.md)。

-   准备待查询的索引数据。

    具体操作，请参见[快速开始](/cn.zh-CN/Elasticsearch/快速开始.md)。


Query DSL是一个Java开源框架，用于构建安全类型的SQL查询语句，能够使用API代替传统的拼接字符串来构造查询语句。目前Query DSL支持的平台包括JPA、JDO、SQL、Java Collections、RDF、Lucene以及Hibernate Search。

Elasticsearch提供了一整套基于JSON的DSL查询语言来定义查询。Query DSL是由一系列抽象的查询表达式组成，特定查询能够包含其它的查询（如bool），部分查询能够包含过滤器（如constant\_score），还有的可以同时包含查询和过滤器（如 filtered）。您可以从Elasticsearch支持的查询集合里面选择任意一个查询表达式，或者从过滤器集合里面选择任意一个过滤器进行组合，构造出复杂的查询。但编写DSL容易出错，仅有少数专业程序人员精通，QueryBuilder能够帮助对Elasticsearch DSL不甚了解或者想提升编写效率的用户快速生成DSL。

![BSearch-QueryBuilder插件背景信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6056359951/p49225.png)

BSearch-QueryBuilder插件具有如下特性：

-   简单易用：BSearch-QueryBuilder插件提供了可视化的界面点选操作来构造Elasticsearch的DSL查询请求，无编码即可完成自定义条件的数据查询，减少了复杂的DSL的学习成本。也可辅助开发人员编写或验证DSL语句的正确性。
-   方便快捷：已经定义的复杂查询条件会保存在Kibana中，避免重复构造查询条件。
-   小巧轻盈：约占用14MB的磁盘空间，不会常驻内存运行，不影响Kibana和Elasticsearch的正常运行。
-   安全可靠：BSearch-QueryBuilder插件不会对用户的数据进行改写、存储和转发，源代码已经通过了阿里云安全审计。

**说明：** 仅6.3和6.7版本的阿里云Elasticsearch实例支持BSearch-QueryBuilder插件。

## 操作步骤

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

4.  在**Discover**页面，单击右上角菜单栏的**Query**。

5.  在查询区域添加查询和过滤条件，单击**Submit**（提交）。

    ![查询结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7056359951/p49357.png)

    -   单击![查询条件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7056359951/p49385.png)图标，添加一个查询条件。
    -   单击![添加一个子过滤条件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7056359951/p49386.png)图标，为查询添加一个子过滤条件。
    -   单击![删除一个查询或过滤条件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7056359951/p49391.png)图标，删除一个查询或过滤条件。
    BSearch-QueryBuilder支持模糊查询、多条件组合查询和自定义时间范围查询等多种查询方式，查询示例如下：

    -   模糊查询

        下图中表示对**email**这个条件进行模糊查询，并要求**email**模糊匹配**iga**。

        ![模糊查询](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7056359951/p49284.png)

        最终得到的匹配结果如下。

        ![查询结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7056359951/p49285.png)

    -   多条件组合查询

        下图的查询条件表示**index**必须为**tryme\_book**，同时要对**type**进行过滤。要求**type**等于**大学教辅**、**数学**、**对外汉语教学**或**大学教材**。

        ![多条件组合查询 ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7056359951/p49286.png)

        最终得到的匹配结果如下。

        ![匹配结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7056359951/p49287.png)

    -   自定义时间范围查询

        当您需要对时间字段进行筛选时，可使用时间类型的筛选功能。下图中对**utc\_time**进行时间范围筛选，查询`[当前时间-240天,当前时间]`范围内的数据。

        ![自定义时间范围查询 ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7056359951/p49288.png)

        最终得到的匹配结果如下。

        ![匹配结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7056359951/p49289.png)


