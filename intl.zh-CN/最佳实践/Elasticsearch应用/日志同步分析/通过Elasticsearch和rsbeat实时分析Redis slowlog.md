# 通过Elasticsearch和rsbeat实时分析Redis slowlog

Redis是目前流行的高性能key-value数据库，但如果使用不当，很容易出现慢查询。慢查询过多或者一个时间较长（例如20s）的慢查询会导致操作队列（Redis是单进程）堵塞，可能会导致服务不可用。因此您需要实时收集并分析Redis slowlog，在出现问题时快速定位解决。本文介绍如何通过Elasticsearch和rsbeat实时分析Redis slowlog。

通过Elasticsearch和rsbeat实时分析Redis slowlog的原理为：使用rsbeat将Redis slowlog采集到Elasticsearch中，然后在Kibana中进行图形化分析。相关概念说明如下：

-   Elasticsearch：是一个基于Lucene的实时分布式的搜索与分析引擎，是遵从Apache开源条款的一款开源产品，是当前主流的企业级搜索引擎。它提供了一个分布式服务，可以使您快速的近乎于准实时的存储、查询和分析超大数据集，通常被用来作为构建复杂查询特性和需求强大应用的基础引擎或技术。

    阿里云Elasticsearch兼容开源Elasticsearch的功能，以及Security、Machine Learning、Graph、APM等商业功能，致力于数据分析、数据搜索等场景服务，支持5.5.3、6.3.2、6.7.0、6.8.0和7.4.0等版本，并提供了商业插件X-Pack服务。在开源Elasticsearch的基础上提供企业级权限管控、安全监控告警、自动报表生成等功能。本文使用阿里云Elasticsearch进行演示，单击即可[免费试用](https://common-buy.aliyun.com/?spm=a2c4g.11186623.2.20.7f9818738QhPyy&commodityCode=elasticsearchpre&request=%7B%22region%22:%22cn-hangzhou%22,%22es_version%22:%225.5.3_with_X-Pack%22,%22network_type%22:%22VPC%22,%22vs_area%22:%22cn-hangzhou-b%22,%22vpc_id%22:%22vpc-bp170psqmu5is7iml6bq9%22,%22vswitch_id%22:%22vsw-bp1jyxgwodxsb1h9tfbih%22,%22node_spec%22:%22elasticsearch.n4.small%22,%22disk%22:20,%22node_amount%22:2,%22dedicate_master%22:false,%22ord_time%22:%22%5B%5Cn%20%201,%5Cn%20%20%5C%22Month%5C%22,%5Cn%20%20null%5Cn%5D%22%7D)。

-   rsbeat：用来收集和分析Redis慢日志的采集器，详情请参见[rsbeat官方文档](https://github.com/yourdream/rsbeat)。
-   Redis：是一个开源的、基于内存的数据结构存储器，可以用作数据库、缓存和消息中间件，详情请参见[Redis官方说明](https://redis.io/)。

    云数据库Redis版（ApsaraDB for Redis）是兼容开源Redis协议标准、提供内存加硬盘的混合存储方式的数据库服务，基于高可靠双机热备架构及可平滑扩展的集群架构，满足高读写性能场景及弹性变配的业务需求。本文使用云数据库Redis版进行演示，更多详情请参见[什么是云数据库Redis版](/intl.zh-CN/产品简介/什么是云数据库Redis版.md)。


## 操作流程

1.  [准备工作](#section_m28_k8s_ntf)

    创建阿里云Elasticsearch实例、云数据库Redis版实例（以下简称Redis实例）和ECS实例，三者在同一专有网络VPC（Virtual Private Cloud）下。

2.  [步骤一：配置Redis慢查询参数](#section_hiv_s7s_9gq)

    根据需求设置Redis slowlog生成的条件，以及可记录的slowlog的最大条数。

3.  [步骤二：安装并配置rsbeat](#section_jj2_pgp_dde)

    在ECS中安装rsbeat，并在其配置文件中指定Redis和Elasticsearch服务。

4.  [步骤三：通过Kibana图形化分析slowlog](#section_gdb_u5k_a4g)

    通过Kibana查看日志详细信息，并根据需求进行统计分析。


## 准备工作

1.  创建阿里云Elasticsearch实例，并开启自动创建索引功能。

    具体操作步骤请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)和[开启自动创建索引](/intl.zh-CN/快速入门/步骤二：配置实例（可选）.md)。本文使用的实例版本为通用商业版6.7。

2.  创建Redis实例。

    具体操作步骤请参见[步骤1：创建实例](/intl.zh-CN/快速入门/步骤1：创建实例.md)。本文使用的实例版本为Redis 5.0社区版，并且与阿里云Elasticsearch实例在同一VPC下，便于内网访问。

3.  创建ECS实例。

    具体操作步骤请参见[使用向导创建实例](/intl.zh-CN/实例/创建实例/使用向导创建实例.md)。本文使用的实例镜像为CentOS 7.6 64位，并且与Redis和Elasticsearch实例在同一VPC下。

4.  配置Redis实例的访问白名单。

    将ECS实例的内网IP地址添加到Redis实例的白名单中，具体操作步骤请参见[设置IP白名单](/intl.zh-CN/用户指南/账号与安全/设置IP白名单.md)。


## 步骤一：配置Redis慢查询参数

1.  登录[Redis管理控制台](https://kvstore.console.aliyun.com/)。

2.  在顶部菜单栏处，选择地域。

3.  在**实例列表**页面，单击目标实例ID或者其右侧**操作**列下的**![更多图标](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1663369951/p163935.png)** \> **管理**。

4.  在左侧导航栏，单击**参数设置**。

5.  在参数设置列表中，找到**slowlog-log-slower-than**和**slowlog-max-len**参数，将其修改为您期望的值。

    |参数|说明|示例|
    |--|--|--|
    |**slowlog-log-slower-than**|当命令执行时间（不包括排队时间）超过该参数值时，该命令会被定义为慢查询，并记录到slowlog中。单位为微秒，默认为10000，即10毫秒。 **说明：** 负数表示关闭慢查询日志功能，0表示记录所有命令操作。

|本文将该参数值设置为20000。表示在slowlog中记录执行时长超过20ms的命令。|
    |**slowlog-max-len**|slowlog中可以记录的最大慢查询命令的条数。当slowlog中的记录数超过最大值后，Redis会将最早的slowlog删除。|本文将该参数值设置为100。表示在slowlog中记录最近100条慢查询命令。|


## 步骤二：安装并配置rsbeat

1.  连接ECS实例。

    具体操作步骤请参见[连接实例](/intl.zh-CN/实例/连接实例/连接Linux实例/通过VNC远程连接登录Linux实例.md)。

2.  安装rsbeat。

    本文使用5.3.2版本。

    ```
    wget https://github.com/Yourdream/rsbeat/archive/master.zip
    unzip master.zip
    ```

3.  修改rsbeat配置。

    1.  执行以下命令打开rsbeat.yml文件。

        ```
        cd rsbeat-master
        vim rsbeat.yml
        ```

    2.  按照以下说明修改rsbeat和output.elasticsearch参数配置，并保存。

        ![rebeat.yml参数配置](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2064659951/p131929.png)

        |参数|说明|
        |--|--|
        |period|每隔多久将slowlog输出到Elasticsearch。|
        |redis|Redis实例的连接地址，获取方式请参见[查看连接地址](/intl.zh-CN/用户指南/连接实例/查看连接地址.md)。 **说明：** 由于配置文件中没有定义Redis实例的密码，因此在获取连接地址后，您还需要开启免密访问，才能确保rsbeat能够访问Redis实例，开启方法请参见[开启免密访问](/intl.zh-CN/用户指南/账号与安全/开启免密访问.md)。 |
        |slowerThan|定义将`config set slowlog-log-slower-than`命令发送到Redis服务器的时间。单位为微秒。|

        |参数|说明|
        |--|--|
        |hosts|阿里云Elasticsearch实例的连接地址，可在实例的基本信息页面获取，详情请参见[查看实例的基本信息](/intl.zh-CN/实例管理/管理实例/查看实例的基本信息.md)。|
        |username|阿里云Elasticsearch实例的访问用户名，默认为elastic。|
        |password|对应用户的密码。elastic用户的密码在创建实例时设定，如果忘记可重置，重置密码的注意事项和操作步骤请参见[重置实例访问密码](/intl.zh-CN/实例管理/安全配置/重置实例访问密码.md)。|
        |template.overwrite|是否覆盖已存在的同名模板，默认为true。|

4.  启动rsbeat服务。

    ```
    ./rsbeat.linux.amd64 -c rsbeat.yml -e -d "*"
    ```


## 步骤三：通过Kibana图形化分析slowlog

1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    具体操作步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。

2.  创建索引模式。

    1.  在左侧导航栏，单击**Management**。

    2.  在**Kibana**区域，单击**Index Patterns**。

    3.  单击**Create index pattern**。

    4.  输入**Index pattern**名称，单击**Next step**。

        ![输入Index pattern](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2064659951/p132004.png)

    5.  从**Time Filter field name**中，选择时间过滤器字段名（本文选择**@timestamp**）。

    6.  单击**Create index pattern**。

3.  查看slowlog的详细信息。

    1.  在左侧导航栏，单击**Discover**。

    2.  在**Discover**页面左侧，选择目标索引模式rsbeat-\*。

    3.  在页面右上角，选择一段时间，查看该时间段内的slowlog信息。

        ![查看slowlog信息](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2064659951/p132012.png)

4.  统计slowlog数量最多的前10个key，并以降序排列展示。

    1.  在左侧导航栏，单击**Visualize**。

    2.  在**Visualize**页面，单击![添加图标](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2064659951/p132045.png)图标。

    3.  在**New Visualization**对话框中，单击**Pie**。

        ![单击Pie](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2064659951/p132052.png)

    4.  选择索引模式rsbeat-\*。

        ![选择索引模式](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2064659951/p132051.png)

    5.  按照下图配置Metrics和Buckets。

        ![Metric和Bucket配置](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3064659951/p132049.png)

    6.  单击![运行图标](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3064659951/p132050.png)图标，查看结果。

        ![统计结果](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3064659951/p132048.png)

        **说明：** 更多Kibana的使用方法，请参见[Kibana官方文档](https://www.elastic.co/guide/en/kibana/6.7/index.html)。


