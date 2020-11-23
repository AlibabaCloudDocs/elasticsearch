---
keyword: Canal将MySQL数据同步至es
---

# 通过Canal将MySQL数据同步到阿里云Elasticsearch

Canal是阿里巴巴集团提供的一个开源产品，能够通过解析数据库的增量日志，提供增量数据的订阅和消费功能。当您需要将MySQL中的增量数据同步至阿里云Elasticsearch时，可通过Canal来实现。本文以阿里云RDS MySQL为例，介绍具体的实现方法。

## 操作步骤

1.  [准备工作](#section_e69_vic_nli)

    创建RDS MySQL实例、了解Canal、创建阿里云Elasticsearch和ECS实例。

    **说明：** 本文使用的RDS MySQL、阿里云Elasticsearch和ECS在相同的专有网络VPC（Virtual Private Cloud）下。

    -   RDS MySQL：用来存放源数据和增量数据。
    -   Canal：进行数据库日志解析，获取增量变更进行同步，是Github中开源的ETL（Extract Transform Load）软件，详情请参见[Canal](https://github.com/alibaba/canal)。
    -   阿里云Elasticsearch：用来接收增量数据。
    -   阿里云ECS：用来部署Canal-server和Canal-adapter。
2.  [步骤一：准备MySQL数据源](#section_bya_9h5_d4u)

    在RDS MySQL中，准备待同步的数据。

3.  [步骤二：创建索引和mapping](#section_tt7_ab3_ll2)

    在阿里云Elasticsearch实例中，创建索引和Mapping。要求Mapping中定义的字段名称和类型与待同步数据保持一致。

4.  [步骤三：安装JDK](#section_1uk_499_551)

    在使用Canal前，必须先安装JDK，要求版本大于等于1.8.0。

5.  [步骤四：安装并启动Canal-server](#section_8jx_g9s_pto)

    安装Canal-server，然后修改配置文件关联RDS MySQL。Canal-server模拟MySQL集群的一个slave，获取MySQL集群Master节点的二进制日志（binary log），并将日志推送给Canal-adapter。

6.  [步骤五：安装并启动Canal-adapter](#section_2tp_qfz_qw5)

    安装Canal-adapter，然后修改配置文件关联RDS MySQL和Elasticsearch，以及定义MySQL数据到Elasticsearch数据的映射字段，用来将数据同步到Elasticsearch。

7.  [步骤六：验证增量数据同步](#section_fxm_zna_2n1)

    在RDS MySQL中新增、修改或删除数据，查看数据同步结果。


## 准备工作

-   创建RDS MySQL实例。

    具体操作步骤请参见[创建RDS MySQL实例](/intl.zh-CN/RDS MySQL 数据库/快速入门/创建RDS MySQL实例.md)。本文使用的配置如下。

    ![RDS for MySQL配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3502659951/p59221.png)

-   创建阿里云Elasticsearch实例。

    具体操作步骤请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。本文创建的实例的**版本**为**通用商业版6.7**。

    **说明：** Canal不支持Elasticsearch 7.0及以上版本。

-   创建阿里云ECS实例。

    具体操作步骤请参见[步骤一：创建ECS实例](/intl.zh-CN/快速入门/通过控制台创建实例（详细版）/Linux系统实例快速入门.md)。本文创建的实例的**镜像**为**CentOS 7.6 64位**。


## 步骤一：准备MySQL数据源

进入[RDS控制台](https://rdsnext.console.aliyun.com)，创建RDS MySQL数据库和表。详情请参见[RDS MySQL快速入门](/intl.zh-CN/RDS MySQL 数据库/快速入门/使用流程.md)。

本文创建的表名称为**es\_test**，包含的字段如下所示。

![es_test字段及索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3502659951/p59264.png)

## 步骤二：创建索引和mapping

1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    登录控制台的具体步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**（开发工具）。

3.  在**Console**中执行以下命令创建索引和mapping。

    **说明：** mapping中的字段需要与[步骤一：准备MySQL数据源](#section_bya_9h5_d4u)中创建的字段（名称和类型）保持一致。

    ```
    PUT es_test?include_type_name=true
    {
    
        "settings" : {
          "index" : {
            "number_of_shards" : "5",
            "number_of_replicas" : "1"
          }
        },
        "mappings" : {
            "_doc" : {
                "properties" : {
                  "count": {          
                       "type": "text"       
                   },
                  "id": {
                       "type": "integer"
                   },
                    "name" : {
                        "type" : "text",
                       "analyzer": "ik_smart"                   
                    },
                    "color" : {
                        "type" : "text"                    
                    }
                }
            }
        }
    }
    ```

    创建成功后，返回如下结果。

    ```
    {
      "acknowledged" : true,
      "shards_acknowledged" : true,
      "index" : "es_test"
    }
    ```


## 步骤三：安装JDK

1.  在阿里云ECS实例中，查看可用的JDK软件包列表。

    ```
    yum search java | grep -i --color JDK
    ```

2.  选择合适的版本，安装JDK。

    本文选择**java-1.8.0-openjdk-devel.x86\_64**。

    ```
    yum install java-1.8.0-openjdk-devel.x86_64
    ```

3.  配置环境变量。

    1.  打开etc文件夹下的profile文件。

        ```
        vi /etc/profile
        ```

    2.  在文件内添加如下的环境变量。

        ```
        export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.71-2.b15.el7_2.x86_64
        export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
        export PATH=$PATH:$JAVA_HOME/bin
        ```

    3.  按下**Esc**键，然后使用`:wq`保存文件并退出vi模式，并执行以下命令使配置生效。

        ```
        source /etc/profile
        ```

4.  分别执行以下命令，验证JDK是否安装成功。

    -   `java`
    -   `javac`
    -   `java -version`
    显示如下结果说明JDK安装成功。

    ![jdk安装成功](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3502659951/p59240.png)


## 步骤四：安装并启动Canal-server

1.  下载Canal-server。

    本文使用1.1.4版本。

    ```
    wget https://github.com/alibaba/canal/releases/download/canal-1.1.4/canal.deployer-1.1.4.tar.gz
    ```

2.  解压。

    ```
    tar -zxvf canal.deployer-1.1.4.tar.gz
    ```

3.  修改conf/example/instance.properties文件。

    ```
    vi conf/example/instance.properties
    ```

    ![修改conf/example/instance.properties文件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3502659951/p59251.png)

    |配置项|说明|
    |---|--|
    |`canal.instance.master.address`|`<RDS MySQL数据库的内网地址>:<内网端口>`，相关信息可在RDS MySQL实例的**基本信息**页面获取。例如`rm-bp1u1xxxxxxxxx6ph.mysql.rds.aliyuncs.com:3306`。|
    |`canal.instance.dbUsername`|RDS MySQL数据库的账号名称，可在实例的**账号管理**页面获取。|
    |`canal.instance.dbPassword`|RDS MySQL数据库的密码。|

4.  按下**Esc**键，然后使用`:wq`命令保存文件并退出vi模式。

5.  启动Canal-server，并查看日志。

    ```
    ./bin/startup.sh
    cat logs/canal/canal.log
    ```

    ![启动canal-server](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3502659951/p59254.png)


## 步骤五：安装并启动Canal-adapter

1.  下载Canal-adapter。

    本文使用1.1.4版本。

    ```
    wget https://github.com/alibaba/canal/releases/download/canal-1.1.4/canal.adapter-1.1.4.tar.gz
    ```

2.  解压。

    ```
    tar -zxvf canal.adapter-1.1.4.tar.gz
    ```

3.  修改conf/application.yml文件。

    ```
    vi conf/application.yml
    ```

    ![修改conf/application.yml文件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3502659951/p59278.png)

    |配置项|说明|
    |---|--|
    |`canal.conf.canalServerHost`|canalDeployer访问地址。保持默认（`127.0.0.1:11111`）即可。|
    |`canal.conf.srcDataSources.defaultDS.url`|`jdbc:mysql://<RDS MySQL内网地址>:<内网端口>/<数据库名称>?useUnicode=true`，相关信息可在RDS MySQL实例的**基本信息**页面获取。例如`jdbc:mysql://rm-bp1xxxxxxxxxnd6ph.mysql.rds.aliyuncs.com:3306/elasticsearch?useUnicode=true`。|
    |`canal.conf.srcDataSources.defaultDS.username`|RDS MySQL数据库的账号名称，可在RDS MySQL实例的**账号管理**页面获取。|
    |`canal.conf.srcDataSources.defaultDS.password`|RDS MySQL数据库的密码。|
    |`canal.conf.canalAdapters.groups.outerAdapters.hosts`|定位到`name:es`的位置，将`hosts`替换为`<Elasticsearch实例的内网地址>:<内网端口>`，相关信息可在Elasticsearch实例的基本信息页面获取。例如`es-cn-v64xxxxxxxxx3medp.elasticsearch.aliyuncs.com:9200`。|
    |`canal.conf.canalAdapters.groups.outerAdapters.mode`|必须设置为`rest`。|
    |`canal.conf.canalAdapters.groups.outerAdapters.properties.security.auth`|`<Elasticsearch实例的账号>:<密码>`。例如`elastic:es_password`。|
    |`canal.conf.canalAdapters.groups.outerAdapters.properties.cluster.name`|Elasticsearch实例的ID，可在实例的基本信息页面获取。例如`es-cn-v64xxxxxxxxx3medp`。|

4.  按下**Esc**键，然后使用`:wq`命令保存文件并退出vi模式。

5.  同样的方式，修改conf/es/\*.yml文件，定义MySQL数据到Elasticsearch数据的映射字段。

    ![修改conf/es/*.yml文件](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4502659951/p59280.png)

    |配置项|说明|
    |---|--|
    |`esMapping._index`|[步骤二：创建索引和mapping](#section_tt7_ab3_ll2)章节中，在Elasticsearch实例中所创建的索引的名称。本文使用**es\_test**。|
    |`esMapping._type`|[步骤二：创建索引和mapping](#section_tt7_ab3_ll2)章节中，在Elasticsearch实例中所创建的索引的类型。本文使用**\_doc**。|
    |`esMapping._id`|需要同步到Elasticsearch实例的文档的id，可自定义。本文使用**\_id**。|
    |`esMapping.sql`|SQL语句，用来查询需要同步到Elasticsearch中的字段。本文使用`select t.id as _id,t.id,t.count,t.name,t.color from es_test t`。|

6.  启动Canal-adapter服务，并查看日志。

    ```
    ./bin/startup.sh
    cat logs/adapter/adapter.log
    ```

    服务启动正常时，结果如下所示。

    ![Canal-adapter服务日志](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4502659951/p59329.png)


## 步骤六：验证增量数据同步

1.  在RDS MySQL数据库中，新增、修改或删除数据库中**es\_test**表的数据。

    ```
    insert `elasticsearch`.`es_test`(`count`,`id`,`name`,`color`) values('11',2,'canal_test2','red');
    ```

2.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    具体步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。

3.  在**Dev Tools**（开发工具）页面的**Console**中，执行以下命令查询同步成功的数据。

    ```
    GET /es_test/_search
    ```

    数据同步成功后，返回如下结果。

    ![数据同步成功结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4502659951/p59328.png)


