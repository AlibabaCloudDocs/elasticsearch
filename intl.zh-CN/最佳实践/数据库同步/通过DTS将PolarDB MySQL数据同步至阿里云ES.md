# 通过DTS将PolarDB MySQL数据同步至阿里云ES

当您在使用PolarDB MySQL遇到查询慢的问题时，可以通过数据传输服务DTS（Data Transmission Service），将企业线上的PolarDB MySQL中的生产数据实时同步到阿里云Elasticsearch（简称ES）中进行搜索分析。本文介绍具体的实现方法。

本案例需要使用以下三个云产品，相关介绍如下：

-   数据传输服务DTS是一种集数据迁移、数据订阅及数据实时同步于一体的数据传输服务，详情请参见[数据传输服务DTS](https://www.alibabacloud.com/product/data-transmission-service)。DTS支持同步的SQL操作包括：Insert、Delete、Update。

    **说明：** 进行数据同步时，请选择DTS支持的数据源及其版本，详情请参见[支持的数据库、同步初始化类型和同步拓扑](/intl.zh-CN/数据同步/支持的数据库、同步初始化类型和同步拓扑.md)。

-   PolarDB是阿里云自研的下一代关系型云数据库，有三个独立的引擎，分别可以100%兼容MySQL、100%兼容PostgreSQL、高度兼容Oracle语法。存储容量最高可达100TB，单库最多可扩展到16个节点，适用于企业多样化的数据库应用场景，详情请参见[PolarDB MySQL概述](/intl.zh-CN/用户指南/概述.md)。
-   Elasticsearch是一个基于Lucene的实时分布式的搜索与分析引擎，它提供了一个分布式服务，可以使您快速的近乎于准实时的存储、查询和分析超大数据集，通常被用来作为构建复杂查询特性和需求强大应用的基础引擎或技术，详情请参见[什么是阿里云Elasticsearch](/intl.zh-CN/产品简介/什么是阿里云Elasticsearch.md)。

本文适用的场景：对实时同步要求较高的关系型数据库中数据的同步场景。

## 注意事项

-   DTS在执行全量数据初始化时将占用源库和目标库一定的读写资源，可能会导致数据库的负载上升，在数据库性能较差、规格较低或业务量较大的情况下（例如源库有大量慢SQL、存在无主键表或目标库存在死锁等），可能会加重数据库压力，甚至导致数据库服务不可用。因此您需要在执行数据同步前评估源库和目标库的性能，同时建议您在业务低峰期执行数据同步（例如源库和目标库的CPU负载在30%以下）。
-   DTS不支持同步DDL操作，如果源库中待同步的表在同步的过程中，已经执行了DDL操作，您需要先移除同步对象，然后在Elasticsearch实例中移除该表对应的索引，最后新增同步对象。详情请参见[移除同步对象](/intl.zh-CN/数据同步/同步作业管理/移除同步对象.md)和[新增同步对象](/intl.zh-CN/数据同步/同步作业管理/新增同步对象.md)。
-   如果源库中待同步的表需要执行增加列的操作，您只需先在Elasticsearch实例中修改对应表的mapping，然后在源库中执行相应的DDL操作，最后暂停并启动DTS同步实例即可。

## 准备工作

-   创建阿里云Elasticsearch实例，并开启实例的自动创建索引功能。

    具体操作步骤请参见[步骤一：创建实例](/intl.zh-CN/Elasticsearch/快速开始.md)和[配置YML参数](/intl.zh-CN/Elasticsearch/ES集群配置/配置YML参数.md)。本文使用6.7版本的实例。

    **说明：** 阿里云Elasticsearch为了保证用户操作数据的安全性，默认把自动创建索引的配置设置为不允许。通过DTS同步数据的时候，使用的是提交数据的方式创建索引，而不是[Create index API](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html)的方式。所以在使用数据同步之前，需要先开启集群的自动创建索引功能。

-   创建云数据库PolarDB MySQL集群，并开启Binlog。

    具体操作步骤请参见[购买按量付费集群](/intl.zh-CN/产品价格与购买/购买步骤/购买按量付费集群.md)、[开启Binlog](/intl.zh-CN/内核功能/其它/开启Binlog.md)。

-   创建PolarDB MySQL数据库和表，并插入测试数据。

    具体操作步骤请参见[管理数据库](/intl.zh-CN/用户指南/账号和数据库/管理数据库.md)。本文使用的表结构和测试数据如下。

    ![测试数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7502659951/p113801.png)

    -   建表语句

        ```
        CREATE TABLE `product` (
            `id` bigint(32) NOT NULL AUTO_INCREMENT,
            `name` varchar(32) NULL,
            `price` varchar(32) NULL,
            `code` varchar(32) NULL,
            `color` varchar(32) NULL,
            PRIMARY KEY (`id`)
        ) ENGINE=InnoDB
        DEFAULT CHARACTER SET=utf8;
        ```

    -   测试数据

        ```
        INSERT INTO `estest`.`product` (`id`,`name`,`price`,`code`,`color`) VALUES (1,'mobile phone A','2000','amp','golden');
        INSERT INTO `estest`.`product` (`id`,`name`,`price`,`code`,`color`) VALUES (2,'mobile phone B','2200','bmp','white');
        INSERT INTO `estest`.`product` (`id`,`name`,`price`,`code`,`color`) VALUES (3,'mobile phone C','2600','cmp','black');
        INSERT INTO `estest`.`product` (`id`,`name`,`price`,`code`,`color`) VALUES (4,'mobile phone D','2700','dmp','red');
        INSERT INTO `estest`.`product` (`id`,`name`,`price`,`code`,`color`) VALUES (5,'mobile phone E','2800','emp','silvery');
        ```


## 操作流程

1.  [准备工作](#section_tu3_vs4_104)

    完成创建ES实例、创建PolarDB MySQL集群、准备测试数据等任务。

2.  [步骤一：配置并启动数据同步链路](#section_k6v_kvf_ttj)

    通过数据传输服务DTS，快速创建并启动PolarDB MySQL到阿里云ES的实时同步作业。

3.  [步骤二：查看数据同步结果](#section_loq_0n9_ab3)

    在阿里云ES的Kibana控制台中查看同步成功的数据。

4.  [步骤三：验证增量数据同步](#section_5kk_uel_2m2)

    验证在PolarDB MySQL数据库中新增数据时，数据的同步效果。


## 步骤一：配置并启动数据同步链路

1.  创建DTS数据同步作业。

    1.  登录[数据传输控制台](https://dts-intl.console.aliyun.com/)。

    2.  在左侧导航栏，单击**数据同步**。

    3.  单击**创建同步作业**，在购买页面按照提示购买同步作业。

        具体步骤请参见[购买数据同步作业]()。购买时选择源实例为**POLARDB**，目标实例为**Elasticsearch**，并选择同步拓扑为**单向同步**。

2.  返回数据同步页面后，选择实例所在地域，然后单击目标实例右侧**操作**列下的**配置同步链路**。

3.  在**创建同步作业**页面，选择同步通道的源及目标实例。

    ![选择源和目标实例](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7502659951/p113780.png)

    |配置项目|配置选项|说明|
    |:---|:---|:-|
    |**同步作业名称**|无|    -   DTS为每个任务自动生成一个同步作业名称，该名称没有唯一性要求。
    -   建议配置具有业务意义的名称，便于后续的识别。 |
    |**源实例信息**|**实例类型**|固定为**PolarDB实例**，不可变更。|
    |**实例地区**|购买数据同步实例时选择的源实例地域信息，不可变更。|
    |**PolarDB实例ID**|选择需要进行数据同步的PolarDB MySQL集群。|
    |**数据库账号**|填入待同步的PolarDB MySQL数据库的账号。 **说明：** 该账号需具备同步对象所属数据库的读权限。 |
    |**数据库密码**|填入数据库账号对应的密码。|
    |**目标实例信息**|**实例类型**|固定为**Elasticsearch**，不可变更|
    |**实例地区**|购买数据同步实例时选择的目标实例地域信息，不可变更。|
    |**Elasticsearch**|选择ES实例ID。|
    |**数据库账号**|填入连接ES实例的账号，默认账号为elastic。|
    |**数据库密码**|填入**数据库账号**对应的密码。|

4.  单击**授权白名单并进入下一步**，等待同步账号创建完成后，单击**下一步**。

    **说明：** 此步骤会将DTS服务器的IP地址自动添加到源PolarDB MySQL实例和目标ES实例的白名单中，用于保障DTS服务器能够正常连接源和目标实例。

5.  选择同步对象。

    ![选择同步对象](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7502659951/p113805.png)

    |配置|说明|
    |:-|:-|
    |**索引名称**|    -   **表名**

选择为**表名**后，在目标Elasticsearch实例中创建的索引名称和表名一致。

    -   **库名\_表名**

选择为**库名\_表名**后，在目标Elasticsearch实例中创建的索引名称为库名\_表名。 |
    |**目标已存在表的处理模式**|    -   **预检查并报错拦截**：检查目标数据库中是否有同名的索引。如果目标数据库中没有同名的索引，则通过该检查项目；如果目标数据库中有同名的索引，则在预检查阶段提示错误，数据同步作业不会被启动。

**说明：** 如果目标库中同名的索引不方便删除或重命名，您可以设置同步对象在目标实例中的名称避免表名冲突，详情请参见[设置同步对象在目标实例中的名称](/intl.zh-CN/数据同步/同步作业管理/设置同步对象在目标实例中的名称.md)。

    -   **忽略报错并继续执行**：跳过目标数据库中是否有同名索引的检查项。

**警告：** 选择**忽略报错并继续执行**，可能导致数据不一致，给业务带来风险，例如：

        -   mapping结构一致的情况下，如果在目标库遇到与源库主键的值相同的记录，在初始化阶段会保留目标库中的该条记录；在增量同步阶段则会覆盖目标库的该条记录。
        -   mapping结构不一致的情况下，可能会导致无法初始化数据、只能同步部分列的数据或同步失败。 |
    |**选择同步对象**|在**源库对象**框中单击待同步的对象，然后单击![向右小箭头](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8502659951/p40698.png)将其移动至**已选择对象**框。 |

6.  在**已选择对象**区域框中，将鼠标指针放置在待同步的表上，并单击表名后出现的**编辑**，设置该表在目标ES实例中的索引名称、Type名称等信息，然后单击**确定**。

    ![编辑表名](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8502659951/p113809.png)

    |配置|说明|
    |--|--|
    |**索引名称**|自定义索引名称，详情请参见[基本概念](/intl.zh-CN/产品简介/基本概念.md)。 **说明：** 输入索引名称时，请确保Elasticsearch集群中不存在同名索引，否则报错`index already exists`。 |
    |**Type名称**|自定义索引类型名称，详情请参见[基本概念](/intl.zh-CN/产品简介/基本概念.md)。|
    |**过滤条件**|您可以设置SQL过滤条件，过滤待同步的数据，只有满足过滤条件的数据才会被同步到目标实例，详情请参见[通过SQL条件过滤待同步数据](/intl.zh-CN/数据同步/同步作业管理/通过SQL条件过滤待同步数据.md)。|
    |**是否分区**|选择是否设置分区，如果选择为**是**，还需要自定义设置**分区列**和**分区数量**（分片数）。|
    |**\_id取值**|    -   **表的主键列**

联合主键合并为一列。

    -   **业务主键**

如果选择为**业务主键**，那么您还需要设置对应的**业务主键列**。 |
    |**添加参数**|选择所需的**字段参数**和**字段参数值**，字段参数及取值介绍请参见[Mapping parameters](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-params.html)。 **说明：** 如果添加参数时，将对应参数的index值设置为false，那么该字段将不能被查询，详情请参见[index](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-index.html#mapping-index)。 |

7.  在页面右下角，单击**预检查并启动**。

    **说明：**

    -   在数据同步作业正式启动之前，系统会先进行预检查。只有预检查通过后，才能成功启动数据同步作业。
    -   如果预检查失败，可单击对应检查项后的![提示](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8502659951/p47468.png)图标，查看失败详情。根据提示修复后，重新进行预检查。
8.  当**预检查**对话框中显示预检查通过后，关闭**预检查**对话框。

    关闭后，同步作业将正式开始。等待同步作业的链路初始化完成，直至处于**同步中**状态后，数据开始同步。

    ![查看作业同步状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8502659951/p113817.png)

    **说明：** 由于PolarDB MySQL和ES实例支持的数据类型不同，数据类型无法一一对应。所以DTS在进行结构初始化时，会根据目标库支持的数据类型进行类型映射，详情请参见[结构初始化涉及的数据类型映射关系](/intl.zh-CN/数据同步/结构初始化涉及的数据类型映射关系.md)。


## 步骤二：查看数据同步结果

1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    登录控制台的具体步骤请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  通过命令查看同步成功的数据。

    1.  在左侧导航栏，单击**Dev Tool**（开发工具）。

    2.  在**Console**中，执行如下命令查看同步成功的数据。

        ```
        GET /product/_doc/_search
        ```

        执行成功后，返回结果如下。

        ![查询返回结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8502659951/p113827.png)

3.  通过控制台操作查看同步成功的数据。

    1.  为目标索引创建索引模式。

        1.  在左侧导航栏，单击**Management**。
        2.  在**Kibana**区域，单击**Index Patterns**。
        3.  单击**Create index pattern**。
        4.  在**Create index pattern**页面的**Index pattern**中，输入索引模式名称。
        5.  单击**Next step**。
        6.  单击**Create index pattern**。
    2.  在左侧导航栏，单击**Discover**。

    3.  选择您已创建的索引模式，查看同步成功的数据。

        ![查看同步成功的数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8502659951/p113868.png)


## 步骤三：验证增量数据同步

1.  进入[PolarDB控制台](https://polardb.console.aliyun.com)。

2.  在PolarDB MySQL数据库中插入一条数据。

    ```
    INSERT INTO `estest`.`product` (`id`,`name`,`price`,`code`,`color`) VALUES (6,'mobile phone F','2750','fmp','white');
    ```

3.  登录Kibana控制台。

    具体步骤请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

4.  在左侧导航栏，单击**Discover**。

5.  选择您已创建的索引模式，查看同步成功的增量数据。

    ![查看同步成功的增量数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8502659951/p113870.png)

    **说明：** 您也可以使用同样的方式，验证在删除或修改PolarDB MySQL数据库中的数据时，数据的同步效果。


