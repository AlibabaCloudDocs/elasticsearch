# 通过DataWorks将Hadoop数据同步至Elasticsearch

当您基于Hadoop进行交互式大数据分析查询，遇到查询延迟的问题时，可以将数据同步至阿里云Elasticsearch中再进行查询分析。Elasticsearch对于多种查询类型，特别是即席查询（Ad Hoc），基本可以达到秒级响应。本文介绍如何通过DataWorks的数据同步功能，将Hadoop数据同步到阿里云Elasticsearch中，并进行搜索分析。

## 操作流程

1.  [准备工作](#section_yzz_in4_vmt)

    搭建Hadoop集群、创建DataWorks工作空间、创建与配置阿里云Elasticsearch实例。

2.  [步骤一：准备数据](#section_y14_epz_42i)

    在Hadoop集群中创建测试数据。

3.  [步骤二：购买并创建独享资源组](#section_j0f_5be_uzv)

    购买并创建一个数据集成独享资源组，并为该资源组绑定专有网络和工作空间。独享资源组可以保障数据快速、稳定地传输。

4.  [步骤三：添加数据源](#section_rh6_39y_kw4)

    将Elasticsearch和Hadoop的HDFS数据源接入DataWorks的数据集成服务中。

5.  [步骤四：配置并运行数据同步任务](#section_p0a_vwy_icq)

    配置一个数据同步的脚本，将数据集成系统同步成功的数据存储到Elasticsearch中。将独享资源组作为一个可以执行任务的资源，注册到DataWorks的数据集成服务中。这个资源组将获取数据源的数据，并执行将数据写入Elasticsearch中的任务（该任务将由数据集成系统统一下发）。

6.  [步骤五：验证数据同步结果](#section_ti9_ca0_mkb)

    在Kibana控制台中，查看同步成功的数据，并按条件查询数据。


## 准备工作

1.  搭建Hadoop集群。

    进行数据同步前，请确保您的Hadoop集群环境正常。本文使用阿里云E-MapReduce服务自动化搭建Hadoop集群，详情请参见[创建集群](/intl.zh-CN/快速入门/创建集群.md)。

    E-MapReduce Hadoop集群配置信息如下（未提到的信息，本文均保持默认，您也可以根据自身需求修改配置）：

    -   **集群类型**：HADOOP
    -   **产品版本**：EMR-3.26.3
    -   **挂载公网**：开启
2.  创建阿里云Elasticsearch实例，并开启实例的自动创建索引功能。

    具体操作步骤请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)和[开启自动创建索引](/intl.zh-CN/快速入门/步骤二：配置实例（可选）.md)。创建实例时，请选择与E-MapReduce集群相同的区域、可用区以及专有网络。本文使用的阿里云Elasticsearch版本为通用商业版6.7.0。

3.  创建DataWorks工作空间。

    创建工作区间时，所选区域需要与阿里云Elasticsearch一致，具体操作步骤请参见[创建工作空间]()。


## 步骤一：准备数据

1.  进入[E-MapReduce控制台](https://emr.console.aliyun.com/)。

2.  在顶部菜单栏，选择地域。

3.  在**集群列表**中，单击目标集群ID。

4.  在上方菜单栏，单击**数据开发**。

5.  在**数据开发**页面，新建一个数据开发项目，其中资源组选择默认资源组。

    具体操作步骤请参见[项目管理](/intl.zh-CN/数据开发/项目管理.md)。

6.  在**项目列表**中，单击目标项目右侧**操作**列下的**作业编辑**，新建一个作业。

    具体操作步骤请参见[作业编辑](/intl.zh-CN/数据开发/作业编辑.md)。其中作业类型选择Hive。

7.  创建数据表并插入数据。

    1.  在代码编辑区域中，输入Hive建表语句，单击**运行**。

        本文档使用的建表语句如下。

        ```
        CREATE TABLE IF NOT
        EXISTS hive_esdoc_good_sale(
         create_time timestamp,
         category STRING,
         brand STRING,
         buyer_id STRING,
         trans_num BIGINT,
         trans_amount DOUBLE,
         click_cnt BIGINT
         )
         PARTITIONED BY (pt string) ROW FORMAT
         DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
        ```

    2.  在**运行作业**对话框中配置运行参数，单击**确定**。

        -   资源组：选择默认资源组。
        -   执行集群：选择您已创建的集群。
    3.  重新新建一个作业，输入如下SQL语句，插入测试数据。

        您可以选择从OSS或其他数据源导入测试数据，也可以手动插入少量的测试数据。本文使用手动插入数据的方法，脚本如下。

        ```
        insert into
        hive_esdoc_good_sale PARTITION(pt =1 ) values('2018-08-21','外套','品牌A','lilei',3,500.6,7),('2018-08-22','生鲜','品牌B','lilei',1,303,8),('2018-08-22','外套','品牌C','hanmeimei',2,510,2),(2018-08-22,'卫浴','品牌A','hanmeimei',1,442.5,1),('2018-08-22','生鲜','品牌D','hanmeimei',2,234,3),('2018-08-23','外套','品牌B','jimmy',9,2000,7),('2018-08-23','生鲜','品牌A','jimmy',5,45.1,5),('2018-08-23','外套','品牌E','jimmy',5,100.2,4),('2018-08-24','生鲜','品牌G','peiqi',10,5560,7),('2018-08-24','卫浴','品牌F','peiqi',1,445.6,2),('2018-08-24','外套','品牌A','ray',3,777,3),('2018-08-24','卫浴','品牌G','ray',3,122,3),('2018-08-24','外套','品牌C','ray',1,62,7) ;
        ```

8.  查看数据是否插入成功。

    1.  新建一个临时查询作业。

        具体操作步骤请参见[临时查询](/intl.zh-CN/数据开发/临时查询.md)。

    2.  输入如下SQL语句，单击**运行**。

        ```
        select * from hive_esdoc_good_sale where pt =1;
        ```

    3.  在页面下方，单击**运行记录**，再单击**操作**列下的**详情**。

    4.  在**运维中心**，单击**作业运行结果**。

        此操作可以检查Hadoop集群表中是否已存在数据可用于同步，运行成功后的结果如下。

        ![查看测试数据](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3824659951/p103731.png)


## 步骤二：购买并创建独享资源组

1.  登录[DataWorks控制台](https://workbench.data.aliyun.com/console)。

2.  选择相应地域后，在左侧导航栏，单击**资源组列表**。

3.  参见[购买独享数据集成资源组]()，购买独享数据集成资源。

    **说明：** 购买时，所选地域需要与目标工作空间保持一致。

4.  参见[新增独享数据集成资源组]()，创建一个独享数据集成资源。

    本文使用的配置如下，其中**资源组类型**选择**独享数据集成资源组**。

    ![创建独享资源组](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3034659951/p134902.png)

5.  单击已创建的独享资源组右侧的**专有网络绑定**，参见[绑定专有网络]()，为该独享资源组绑定专有网络。

    独享资源部署在DataWorks托管的专有网络中。DataWorks需要与Hadoop集群和Elasticsearch实例的专有网络连通才能同步数据。而Hadoop集群和Elasticsearch实例在同一专有网络下，因此在绑定专有网络时，选择Elasticsearch实例所在**专有网络**和**交换机**即可。

    ![绑定专有网络](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p134914.png)

6.  单击已创建的独享资源组右侧的**修改归属工作空间**，参见[修改归属工作空间]()，为该独享资源组绑定目标工作空间。

    ![绑定工作空间](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p139131.png)


## 步骤三：添加数据源

1.  进入DataWorks的**数据集成**页面。

    1.  在DataWorks控制台的左侧导航栏，单击**工作空间列表**。

    2.  找到目标工作空间，单击其右侧**操作**列下的**进入数据集成**。

2.  在左侧导航栏，单击**数据源**。

3.  在**数据源管理**页面，单击**新增数据源**。

4.  在**新增数据源**对话框的**半结构化存储**区域中，单击**HDFS**。

5.  在**新增HDFS数据源**对话框中，填写**数据源名称**和**DefaultFS**。

    ![添加HDFS数据源](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4824659951/p103833.png)

    **DefaultFS**：对于E-MapReduce Hadoop集群而言，如果Hadoop集群为非HA集群，则此处地址为`hdfs://emr-header-1的IP:9000`。如果Hadoop集群为HA集群，则此处地址为`hdfs://emr-header-1的IP:8020`。在本文中，emr-header-1与DataWorks通过专有网络连接，因此此处填写内网IP。

    配置完成后，可与独享资源组进行连通性测试。**连通状态**显示为**可连通**时，表示连通成功。

    ![测试连通成功](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p139093.png)

6.  单击**完成**。

7.  使用同样的方式添加Elasticsearch数据源。

    ![ES数据源配置](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p103880.png)

    |参数|说明|
    |--|--|
    |**Endpoint**|阿里云Elasticsearch的访问地址，格式为：`http://<实例的内网或公网地址>:9200`。实例的内网或公网地址可在基本信息页面获取，详情请参见[查看实例的基本信息](/intl.zh-CN/实例管理/管理实例/查看实例的基本信息.md)。**说明：** 如果您使用的是公网地址，需要将独享资源组的EIP地址添加到阿里云Elasticsearch的公网地址访问白名单中，详情请参见[配置ES公网或私网访问白名单](/intl.zh-CN/实例管理/安全配置/配置ES公网或私网访问白名单.md)和[添加独享数据集成资源组的白名单]()。 |
    |**用户名**|访问阿里云Elasticsearch实例的用户名，默认为elastic。|
    |**密码**|对应用户的密码。elastic用户的密码在创建实例时设定，如果忘记可重置，重置密码的注意事项和操作步骤请参见[重置实例访问密码](/intl.zh-CN/实例管理/安全配置/重置实例访问密码.md)。|

    **说明：** 其他未提及的参数请自定义输入。


## 步骤四：配置并运行数据同步任务

1.  在DataWorks的数据开发页面，新建一个业务流程。

    具体操作步骤请参见[管理业务流程]()。

2.  展开新建的业务流程，右键单击**数据集成**，选择**新建** \> **离线同步**。

3.  在**新建节点**对话框中，输入**节点名称**，单击**提交**。

4.  在页面上方工具栏，单击![转换脚本图标](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p69021.png)图标。

5.  确认后，配置数据同步脚本。

    详细配置说明请参见[通过脚本模式配置任务]()。

    **说明：** 您可以单击页面上方工具栏中的![导入模板](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p87612.png)图标，导入对应的脚本配置模板，并在模板的基础上进行修改，完成数据同步脚本配置。

    示例脚本如下。

    ```
    {
        "order": {
            "hops": [
                {
                    "from": "Reader",
                    "to": "Writer"
                }
            ]
        },
        "setting": {
            "errorLimit": {
                "record": "10"
            },
            "speed": {
                "concurrent": 3,
                "throttle": false
            }
        },
        "steps": [
            {
                "category": "reader",
                "name": "Reader",
                "parameter": {
                    "column": [
                        {
                            "format": "yyyy-MM-dd HH:mm:ss",
                            "index": 0,
                            "type": "date"
                        },
                        {
                            "index": 1,
                            "type": "string"
                        },
                        {
                            "index": 2,
                            "type": "string"
                        },
                        {
                            "index": 3,
                            "type": "string"
                        },
                        {
                            "index": 4,
                            "type": "long"
                        },
                        {
                            "index": 5,
                            "type": "double"
                        },
                        {
                            "index": 6,
                            "type": "long"
                        }
                    ],
                    "datasource": "HDFS_data_source",
                    "encoding": "UTF-8",
                    "fieldDelimiter": ",",
                    "fileType": "text",
                    "path": "/user/hive/warehouse/hive_esdoc_good_sale"
                },
                "stepType": "hdfs"
            },
            {
                "category": "writer",
                "name": "Writer",
                "parameter": {
                    "batchSize": 1000,
                    "cleanup": true,
                    "column": [
                        {
                            "name": "create_time",
                            "type": "id"
                        },
                        {
                            "name": "category",
                            "type": "text"
                        },
                        {
                            "name": "brand",
                            "type": "text"
                        },
                        {
                            "name": "buyer_id",
                            "type": "text"
                        },
                        {
                            "name": "trans_num",
                            "type": "integer"
                        },
                        {
                            "name": "trans_amount",
                            "type": "double"
                        },
                        {
                            "name": "click_cnt",
                            "type": "integer"
                        }
                    ],
                    "datasource": "ES_data_source",
                    "discovery": false,
                    "index": "hive_esdoc_good_sale",
                    "indexType": "_doc",
                    "splitter": ","
                },
                "stepType": "elasticsearch"
            }
        ],
        "type": "job",
        "version": "2.0"
    }
    ```

    同步脚本的配置分为三个部分。

    |配置|说明|
    |--|--|
    |`setting`|用来配置同步中的一些丢包和最大并发等参数。其中`errorLimit`的`record`字段值默认为0，请将其修改为大一些的数值，比如10。|
    |`Reader`|用来配置HDFS Reader。`path`为数据在Hadoop集群中存放的位置，您可以在登录Master节点后，使用`hdfs dfs –ls /user/hive/warehouse/hive_esdoc_good_sale`命令确认。对于分区表，您可以不指定分区，DataWorks数据同步会自动递归到分区路径。详情请参见[HDFS Reader]()。|
    |`Writer`|用来配置Elasticsearch Writer，详情请参见[Elasticsearch Writer]()。     -   `index`：索引名称。
    -   `indexType`：索引类型，7.0及以上版本的Elasticsearch必须使用`_doc`。 |

6.  保存配置脚本，单击右侧的**调度配置**，按照需求配置相应的调度参数。

    各配置的详细说明请参见[调度配置]()章节。

    **说明：**

    -   在提交任务前，必须配置任务调度**依赖的上游节点**，详情请参见[依赖关系]()。
    -   如果您希望对任务进行周期性调度，需要配置任务的**时间属性**，包括任务的具体执行时间、调度周期、生效周期、重跑属性等。
    -   周期任务将于配置任务开始的第二天00:00，按照您的配置规则生效执行。
7.  配置执行同步任务所使用的资源组。

    ![选择资源组](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p69063.png)

    1.  在脚本配置页面右侧，单击**数据集成资源组配置**。

    2.  选择**方案**为**独享数据集成资源组**。

    3.  选择**独享数据集成资源组**为您创建的独享资源组。

8.  提交任务。

    1.  保存当前配置，单击![提交图标](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p71328.png)图标。

    2.  在**提交新版本**对话框中，填入**备注**。

    3.  单击**确认**。

9.  单击![运行图标](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p103889.png)图标，运行任务。

    任务运行过程中，可查看运行日志。运行成功后，显示如下结果。

    ![任务运行成功](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p103890.png)


## 步骤五：验证数据同步结果

1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    具体操作步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**（开发工具）。

3.  在**Console**中，执行如下命令查看同步的数据。

    ```
    POST /hive_esdoc_good_sale/_search?pretty
    {
    "query": { "match_all": {}}
    }
    ```

    **说明：** `hive_esdoc_good_sale`为您在数据同步脚本中设置的`index`字段的值。

    数据同步成功后，返回如下结果。

    ![查看已经同步的数据](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5824659951/p40081.png)

4.  执行如下命令，搜索品牌为A的所有文档。

    ```
    POST /hive_esdoc_good_sale/_search?pretty
    {
      "query": { "match_phrase": { "brand":"品牌A" } }
    }
    ```

    ![返回品牌为A的所有文档](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5824659951/p40082.png)

5.  执行如下命令，按照**点击次数**进行排序，判断各品牌产品的热度。

    ```
    POST /hive_esdoc_good_sale/_search?pretty
    {
    "query": { "match_all": {} },
    "sort": { "click_cnt": { "order": "desc" } },
    "_source": ["category", "brand","click_cnt"]
    }
    ```

    ![按照点击次数进行排序](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5824659951/p40083.png)

    更多命令和访问方式，请参见[阿里云Elasticsearch官方文档](https://www.alibabacloud.com/help/product/57736.htm)和[Elasticsearch官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/index.html)。


