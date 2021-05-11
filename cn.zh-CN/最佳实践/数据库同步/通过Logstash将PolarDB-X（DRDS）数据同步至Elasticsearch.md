# 通过Logstash将PolarDB-X（DRDS）数据同步至Elasticsearch

当您的业务数据存储在PolarDB-X（原DRDS升级版）中，需要进行全文检索和语义分析时，可借助阿里云Elasticsearch实现。本文介绍如何通过阿里云Logstash，将PolarDB-X中的数据实时同步至阿里云Elasticsearch。

PolarDB-X是由阿里巴巴自主研发的云原生分布式数据库，融合分布式SQL引擎DRDS与分布式自研存储X-DB，基于云原生一体化架构设计，可支撑千万级并发规模及百PB级海量存储。专注解决海量数据存储、超高并发吞吐、大表瓶颈以及复杂计算效率等数据库瓶颈问题，历经各届天猫双十一及阿里云各行业客户业务的考验，助力企业加速完成业务数字化转型。详细信息，请参见[PolarDB-X产品概述]()。

阿里云Elasticsearch兼容开源Elasticsearch的功能，以及Security、Machine Learning、Graph、APM等商业功能，致力于数据分析、数据搜索等场景服务。支持5.5.3、6.3.2、6.7.0、6.8.0、7.4.0和7.7.1等版本，并提供了商业插件X-Pack服务。在开源Elasticsearch的基础上提供企业级权限管控、安全监控告警、自动报表生成等功能。详细信息，请参见[什么是阿里云Elasticsearch](/cn.zh-CN/产品简介/什么是阿里云Elasticsearch.md)。单击即可[免费试用](https://common-buy.aliyun.com/new?spm=a2c4g.11186623.2.9.ab722cbePLnzOs&commodityCode=elasticsearchpre&orderType=BUY&regionId=cn-hangzhou)。

阿里云Logstash作为服务器端的数据处理管道，提供了100%兼容开源Logstash的能力。Logstash能够动态地从多个来源采集数据、转换数据，并且将数据存储到所选择的位置。通过输入、过滤和输出插件，Logstash可以对任何类型的事件加工和转换。阿里云Logstash除了支持所有官方预置插件外，还致力于打造包含logstash-input-sls、logstash-input-oss、logstash-output-oss等适用各类场景的插件中心，为您提供更为强大的数据处理和搬迁能力，实现云上数据生态打通。详细信息，请参见[什么是阿里云Logstash](/cn.zh-CN/Logstash/什么是阿里云Logstash.md)。单击即可[免费试用](https://common-buy.aliyun.com/new?spm=a2c0j.9528745.1335467%E2%80%A651f65a368bTZhJ&commodityCode=elasticsearch_logstash_pre&orderType=BUY&accounttraceid=f1c871a1c5f94bef9e54c5dd781ef5ddafdu)。

## 使用限制

通过阿里云Logstash实现PolarDB-X和Elasticsearch数据同步，本质上是使用Logstash的logstash-input-jdbc插件（默认已安装，不可卸载），通过管道配置功能进行数据同步。该插件会定期对PolarDB-X数据库中的数据进行循环轮询，从而在当前循环中找到上次插入或更改的记录。因此要让同步任务正确运行，Elasticsearch和PolarDB-X数据库必须满足以下条件：

-   Elasticsearch中的\_id字段必须与数据库中的id字段相同。

    该条件可以确保当您将数据库中的记录写入Elasticsearch时，同步任务可在数据库记录与Elasticsearch文档之间建立一个直接映射的关系。例如当您在数据库中更新了某条记录时，同步任务会覆盖Elasticsearch中与更新记录具有相同id的文档。

    **说明：** 根据Elasticsearch内部原理，更新操作的本质是删除旧文档然后对新文档进行索引，因此在Elasticsearch中覆盖文档的效率与[更新操作](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/getting-started-update-documents.html)的效率一样高。

-   当您在数据库中插入或者更新数据时，对应记录必须有一个包含更新或插入时间的字段。

    Logstash每次对数据库进行轮询时，都会保存其从数据库中所读取的最后一条记录的更新或插入时间。在读取数据时，Logstash仅读取符合条件的记录，即该记录的更新或插入时间需要晚于上一次轮询中最后一条记录的更新或插入时间。

    **说明：** logstash-input-jdbc插件无法实现同步删除。如果您要删除Elasticsearch中的数据，需要在Elasticsearch中执行相关命令，手动删除。


## 准备工作

1.  创建PolarDB-X 1.0实例、数据库、表，并准备测试数据。

    具体操作，请参见[PolarDB-X 1.0快速入门]()。

    本文使用的建表语句如下。

    ```
    CREATE table food(
    id int PRIMARY key AUTO_INCREMENT,
    name VARCHAR (32),
    insert_time DATETIME,
    update_time DATETIME );
    ```

    插入数据语句如下。

    ```
    INSERT INTO food values(null,'巧克力',now(),now());
    INSERT INTO food values(null,'酸奶',now(),now());
    INSERT INTO food values(null,'火腿肠',now(),now());
    ```

2.  创建阿里云Elasticsearch实例，并开启自动创建索引功能。

    创建实例时，所选专有网络要与PolarDB-X 1.0实例一致。具体操作，请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)和[开启自动创建索引](/cn.zh-CN/Elasticsearch/快速访问与配置.md)。本文使用6.7.0版本的实例。

3.  创建阿里云Logstash实例，并上传与PolarDB-X数据库版本兼容的SQL JDBC驱动。

    创建时所选专有网络和版本要与目标Elasticsearch实例一致。具体操作，请参见[创建阿里云Logstash实例](/cn.zh-CN/Logstash/快速入门/步骤一：创建实例/创建阿里云Logstash实例.md)和[配置扩展文件](/cn.zh-CN/Logstash/集群配置/配置扩展文件.md)。本文使用mysql-connector-java-5.1.35.jar驱动。

    **说明：** 您也可以使用公网环境的服务，前提是需要通过配置NAT网关实现与公网的连通。详细信息，请参见[配置NAT公网数据传输](/cn.zh-CN/Logstash/网络与安全/配置NAT公网数据传输.md)。

4.  在PolarDB-X数据库白名单中加入阿里云Logstash节点的IP地址（可在基本信息页面获取）。

    具体操作，请参见[设置白名单]()。


## 配置Logstash管道

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Logstash实例**。

3.  在顶部菜单栏处，选择地域，然后在**Logstash实例**中单击目标实例ID。

4.  单击左侧导航栏的**管道管理**。

5.  在**管道列表**区域，单击**创建管道**。

    ![创建管道](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2312659951/p95025.png)

6.  在**创建管道任务**页面，输入**管道ID**，并进行Config配置。

    本文使用的Config配置如下。

    ```
    input {
      jdbc {
        jdbc_driver_class => "com.mysql.jdbc.Driver"
        jdbc_driver_library => "/ssd/1/share/<Logstash实例ID>/logstash/current/config/custom/mysql-connector-java-5.1.35.jar"
        jdbc_connection_string => "jdbc:mysql://drdshbga51x6****.drds.aliyuncs.com:3306/<数据库名称>?useUnicode=true&characterEncoding=utf-8&useSSL=false&allowLoadLocalInfile=false&autoDeserialize=false"
        jdbc_user => "db_user"
        jdbc_password => "db_password"
        jdbc_paging_enabled => "true"
        jdbc_page_size => "50000"
        statement => "select * from food where update_time >= :sql_last_value"
        schedule => "* * * * *"
        record_last_run => true
        last_run_metadata_path => "/ssd/1/<Logstash实例ID>/logstash/data/last_run_metadata_update_time.txt"
        clean_run => false
        tracking_column_type => "timestamp"
        use_column_value => true
        tracking_column => "update_time"
      }
    }
    filter {
    
    }
    output {
      elasticsearch {
        hosts => "http://es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com:9200"
        user => "elastic"
        password => "es_password"
        index => "drds_test"
        document_id => "%{id}"
      }
    }
    ```

    **说明：** 代码中`<Logstash实例ID>`需要替换为您创建的Logstash实例的ID。获取方式，请参见[实例列表](/cn.zh-CN/Logstash/实例管理/实例列表.md)。

    |参数|描述|
    |--|--|
    |jdbc\_driver\_class|JDBC Class配置。|
    |jdbc\_driver\_library|指定JDBC连接MySQL驱动文件。具体操作请参见[配置扩展文件](/cn.zh-CN/Logstash/集群配置/配置扩展文件.md)。|
    |jdbc\_connection\_string|配置数据库连接的域名、端口及数据库。|
    |jdbc\_user|数据库用户名。|
    |jdbc\_password|数据库密码。|
    |jdbc\_paging\_enabled|是否启用分页，默认false。|
    |jdbc\_page\_size|分页大小。|
    |statement|指定SQL语句。|
    |schedule|指定定时操作，`"* * * * *"`表示每分钟定时同步数据。|
    |record\_last\_run|是否记录上次执行结果。如果为true，则会把上次执行到的tracking\_column字段的值记录下来，保存到last\_run\_metadata\_path指定的文件中。|
    |last\_run\_metadata\_path|指定最后运行时间文件存放的地址。目前后端开放了/ssd/1/<Logstash实例ID\>/logstash/data/路径来保存文件。|
    |clean\_run|是否清除last\_run\_metadata\_path的记录，默认为false。如果为true，那么每次都要从头开始查询所有的数据库记录。|
    |use\_column\_value|是否需要记录某个column的值。|
    |tracking\_column\_type|跟踪列的类型，默认是numeric。|
    |tracking\_column|指定跟踪列，该列必须是递增的，一般是表的主键。|

    |参数|说明|
    |--|--|
    |hosts|阿里云Elasticsearch实例的访问地址，格式为http://<实例的内网地址\>:9200。其中实例的内网地址可在基本信息页面获取，详细信息请参见[查看实例的基本信息](/cn.zh-CN/Elasticsearch/实例管理/查看实例的基本信息.md)。|
    |user|访问阿里云Elasticsearch实例的用户名，默认为elastic。|
    |password|对应用户的密码。elastic用户的密码在创建实例时设定，如果忘记可重置。重置密码的注意事项和操作，请参见[重置实例访问密码](/cn.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|
    |index|索引名称。|
    |document\_id|文档ID。设置为%\{id\}，表示与源数据库中的id字段保持一致。|

    **说明：**

    -   以上配置按照测试数据配置，在实际业务中，请按照业务需求进行合理配置。input插件支持的其他配置选项，请参见官方[Logstash Jdbc input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-jdbc.html#plugins-inputs-jdbc-common-options)
    -   如果配置中有类似`last_run_metadata_path`的参数，需要阿里云Logstash服务提供文件路径。目前后端开放了`/ssd/1/<Logstash实例ID>/logstash/data/`路径供您测试使用，且该目录下的数据不会被删除。因此在使用时，请确保磁盘有充足的使用空间。
    -   为了提升安全性，如果在配置管道时使用了JDBC驱动，需要在`jdbc_connection_string`参数后面添加`allowLoadLocalInfile=false&autoDeserialize=false`，否则当您在添加Logstash配置文件时，调度系统会抛出校验失败的提示，例如`jdbc_connection_string => "jdbc:mysql://drdshbga51x6****.drds.aliyuncs.com:3306/<数据库名称>?allowLoadLocalInfile=false&autoDeserialize=false"`。
    更多Config配置，请参见[Logstash配置文件说明](/cn.zh-CN/Logstash/管道任务管理/Logstash配置文件说明.md)。

7.  单击**下一步**，配置管道参数。

    ![管道参数配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2312659951/p67293.png)

    |参数|说明|
    |--|--|
    |**管道工作线程**|并行执行管道的Filter和Output的工作线程数量。当事件出现积压或CPU未饱和时，请考虑增大线程数，更好地使用CPU处理能力。默认值：实例的CPU核数。|
    |**管道批大小**|单个工作线程在尝试执行Filter和Output前，可以从Input收集的最大事件数目。较大的管道批大小可能会带来较大的内存开销。您可以设置LS\_HEAP\_SIZE变量，来增大JVM堆大小，从而有效使用该值。默认值：125。|
    |**管道批延迟**|创建管道事件批时，将过小的批分派给管道工作线程之前，要等候每个事件的时长，单位为毫秒。默认值：50ms。|
    |**队列类型**|用于事件缓冲的内部排队模型。可选值：     -   **MEMORY**：默认值。基于内存的传统队列。
    -   **PERSISTED**：基于磁盘的ACKed队列（持久队列）。 |
    |**队列最大字节数**|请确保该值小于您的磁盘总容量。默认值：1024 MB。|
    |**队列检查点写入数**|启用持久性队列时，在强制执行检查点之前已写入事件的最大数目。设置为0，表示无限制。默认值：1024。|

    **警告：** 配置完成后，需要**保存并部署**才能生效。**保存并部署**操作会触发实例重启，请在不影响业务的前提下，继续执行以下步骤。

8.  单击**保存**或者**保存并部署**。

    -   **保存**：将管道信息保存在Logstash里并触发实例变更，配置不会生效。保存后，系统会返回**管道管理**页面。可在**管道列表**区域，单击**操作**列下的**立即部署**，触发实例重启，使配置生效。
    -   **保存并部署**：保存并且部署后，会触发实例重启，使配置生效。

## 验证结果

1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    具体操作，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**（开发工具）。

3.  在**Console**中，执行以下命令，查看同步成功的索引数量。

    ```
    GET drds_test/_count
    {
      "query": {"match_all": {}}
    }
    ```

    运行成功后，结果如下。

    ```
    {
      "count" : 3,
      "_shards" : {
        "total" : 1,
        "successful" : 1,
        "skipped" : 0,
        "failed" : 0
      }
    }
    ```

4.  在表中更新并插入数据。

    ```
    UPDATE food SET name='Chocolates',update_time=now() where id = 1;
    INSERT INTO food values(null,'鸡蛋',now(),now());
    ```

5.  在Kibana控制台，查看更新后的数据。

    -   查询name为Chocolates的数据。

        ```
        GET drds_test/_search
        {
          "query": {
            "match": {
              "name": "Chocolates"
           }}
        }
        ```

        返回结果如下。

        ![返回结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4602659951/p137899.png)

    -   查询所有数据。

        ```
        GET drds_test/_search
        {
          "query": {
            "match_all": {}
          }
        }
        ```

        返回结果如下。

        ![所有数据的返回结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4602659951/p137901.png)


