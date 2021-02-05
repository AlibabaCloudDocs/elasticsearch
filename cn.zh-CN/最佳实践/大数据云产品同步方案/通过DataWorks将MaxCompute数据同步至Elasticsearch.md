---
keyword: [odps数据同步到阿里云es, MaxCompute数据同步到阿里云es]
---

# 通过DataWorks将MaxCompute数据同步至Elasticsearch

阿里云上拥有丰富的云存储、云数据库产品。当您需要对这些产品中的数据进行分析和搜索时，可以通过DataWorks的数据集成服务实现最快5分钟一次的离线数据采集，并同步到阿里云Elasticsearch中。本文以阿里云大数据计算服务MaxCompute（原名ODPS）为例。

阿里云Elasticsearch支持同步的离线数据源包括：

-   阿里云云数据库（MySQL、PostgreSQL、SQL Server、PPAS、MongoDB、HBase）
-   阿里云PolarDB-X（原DRDS升级版）
-   阿里云MaxCompute
-   阿里云OSS
-   阿里云Tablestore
-   自建HDFS、Oracle、FTP、DB2，及以上数据库的自建版本

## 操作流程

1.  [准备工作](#section_cx6_k3k_xsa)

    创建DataWorks工作空间并开通MaxCompute服务、准备MaxCompute数据源、创建阿里云Elasticsearch实例。

2.  [步骤一：购买并创建独享资源组](#section_5u1_9vc_phf)

    购买并创建一个数据集成独享资源组，并为该资源组绑定专有网络和工作空间。独享资源组可以保障数据快速、稳定地传输。

3.  [步骤二：添加数据源](#section_enz_k2e_5on)

    将MaxCompute和Elasticsearch数据源接入DataWorks的数据集成服务中。

4.  [步骤三：配置并运行数据同步任务](#section_qr9_459_75g)

    配置一个数据同步的脚本，将数据集成系统同步成功的数据存储到Elasticsearch中。将独享资源组作为一个可以执行任务的资源，注册到DataWorks的数据集成服务中。这个资源组将获取数据源的数据，并执行将数据写入Elasticsearch中的任务（该任务将由数据集成系统统一下发）。

5.  [步骤四：验证数据同步结果](#section_v84_pvy_wz7)

    在Kibana控制台中，查看同步成功的数据，并按条件查询数据。


## 准备工作

1.  创建DataWorks工作空间。创建时选择MaxCompute计算引擎服务。

    具体操作，请参见[创建项目空间](/cn.zh-CN/准备工作/创建项目空间.md)。

2.  创建MaxCompute表并导入测试数据。

    具体操作，请参见[创建和查看表](/cn.zh-CN/快速入门/创建和查看表.md)、[导入数据](/cn.zh-CN/快速入门/导入数据.md)。

    本文使用的表结构和部分数据如下。

    ![表结构](../images/p40112.png "表结构")

    ![表数据](../images/p40113.png "表数据")

    **说明：** 本文提供的数据仅供测试。实际情况中，您可以将Hadoop数据迁移到MaxCompute后再进行同步。详细信息，请参见[Hadoop数据迁移MaxCompute最佳实践](/cn.zh-CN/最佳实践/数据迁移/Hadoop数据迁移MaxCompute最佳实践.md)。

3.  创建阿里云Elasticsearch实例，并开启实例的自动创建索引功能。

    具体操作，请参见[创建阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)和[配置YML参数](/cn.zh-CN/Elasticsearch/集群配置/配置YML参数.md)。创建实例时，所选地域与DataWorks工作空间所在地域保持一致。


## 步骤一：购买并创建独享资源组

1.  登录[DataWorks控制台](https://workbench.data.aliyun.com/console)。

2.  选择相应地域后，在左侧导航栏，单击**资源组列表**。

3.  参见[购买独享数据集成资源组]()，购买独享数据集成资源。

    **说明：** 购买时，所选地域需要与目标工作空间保持一致。

4.  参见[新增独享数据集成资源组]()，创建一个独享数据集成资源。

    本文使用的配置如下，其中**资源组类型**选择**独享数据集成资源组**。

    ![创建独享资源组](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3034659951/p134902.png)

5.  单击已创建的独享资源组右侧的**专有网络绑定**，参见[网络配置]()，为该独享资源组绑定专有网络。

    独享资源部署在DataWorks托管的专有网络中。DataWorks需要与Elasticsearch实例的专有网络连通才能同步数据，因此在绑定专有网络时，需要选择Elasticsearch实例所在**专有网络**和**交换机**。

    ![绑定专有网络](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p134914.png)

6.  单击已创建的独享资源组右侧的**修改归属工作空间**，参见[绑定归属工作空间]()，为该独享资源组绑定目标工作空间。

    ![绑定工作空间](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p139131.png)


## 步骤二：添加数据源

1.  进入DataWorks的**数据集成**页面。

    1.  在DataWorks控制台的左侧导航栏，单击**工作空间列表**。

    2.  找到目标工作空间，单击其右侧**操作**列下的**进入数据集成**。

2.  在左侧导航栏，单击**数据源**。

3.  在**数据源管理**页面，单击**新增数据源**。

4.  在**新增数据源**对话框中，单击**MaxCompute\(ODPS\)**，进入**新增MaxCompute\(ODPS\)数据源**页面，填写数据源信息。

    ![新增MaxCompute (ODPS)数据源](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p40122.png)

    |参数|说明|
    |--|--|
    |**ODPS Endpoint**|MaxCompute服务的访问地址，不同区域的访问地址不同，详情请参见[配置Endpoint](/cn.zh-CN/准备工作/配置Endpoint.md)。|
    |**ODPS项目名称**|进入[DataWorks控制台](https://workbench.data.aliyun.com/console)，在左侧导航栏，单击**计算引擎列表**下的**MaxCompute**获取。|
    |**AccessKey ID**|鼠标移至您的用户头像上，单击**AccessKey 管理**获取。|
    |**AccessKey Secret**|鼠标移至您的用户头像上，单击**AccessKey 管理**获取。|

    **说明：** 以上未提及的配置请自定义输入或保持默认。

    配置完成后，可与独享资源组进行连通性测试。**连通状态**显示为**可连通**时，表示连通成功。

    ![测试连通成功](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p139093.png)

5.  单击**完成**。

6.  使用同样的方式添加Elasticsearch数据源。

    ![ES数据源配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p103880.png)

    |参数|说明|
    |--|--|
    |**Endpoint**|阿里云Elasticsearch的访问地址，格式为：`http://<实例的内网或公网地址>:9200`。实例的内网或公网地址可在基本信息页面获取，详细信息，请参见[查看实例的基本信息](/cn.zh-CN/Elasticsearch/实例管理/查看实例的基本信息.md)。**说明：** 如果您使用的是公网地址，需要将独享资源组的EIP地址添加到阿里云Elasticsearch的公网地址访问白名单中，详情请参见[配置ES公网或私网访问白名单](/cn.zh-CN/Elasticsearch/安全配置/配置ES公网或私网访问白名单.md)和[添加独享数据集成资源组的白名单]()。 |
    |**用户名**|访问阿里云Elasticsearch实例的用户名，默认为elastic。|
    |**密码**|对应用户的密码。elastic用户的密码在创建实例时设定，如果忘记可重置，重置密码的注意事项和操作步骤，请参见[重置实例访问密码](/cn.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|

    **说明：** 其他未提及的参数请自定义输入。


## 步骤三：配置并运行数据同步任务

1.  在DataWorks的数据开发页面，新建一个业务流程。

    具体操作步骤请参见[管理业务流程]()。

2.  展开新建的业务流程，右键单击**数据集成**，选择**新建** \> **离线同步**。

3.  在**新建节点**对话框中，输入**节点名称**，单击**提交**。

4.  在页面上方工具栏，单击![转换脚本图标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p69021.png)图标。

5.  确认后，配置数据同步脚本。

    详细配置说明，请参见[通过脚本模式配置任务]()。

    **说明：** 您可以单击页面上方工具栏中的![导入模板](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p87612.png)图标，导入对应的脚本配置模板，并在模板的基础上进行修改，完成数据同步脚本配置。

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
                "record": "0"
            },
            "speed": {
                "concurrent": 1,
                "throttle": false
            }
        },
        "steps": [
            {
                "category": "reader",
                "name": "Reader",
                "parameter": {
                    "column": [
                        "create_time",
                        "category",
                        "brand",
                        "buyer_id",
                        "trans_num",
                        "trans_amount",
                        "click_cnt"
                    ],
                    "datasource": "odps_es",
                    "partition": "pt=1",
                    "table": "hive_doc_good_sale"
                },
                "stepType": "odps"
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
                    "index": "odps_index",
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
    |`setting`|用来配置同步中的一些丢包和最大并发等参数。 **说明：** 如果待同步的数据量较大，可适当增大最大并发数。 |
    |`Reader`|用来配置Maxcompute Reader。如果您的MaxCompute表是一个分区表，需要在`partition`字段中设置分区信息。详细信息，请参见[MaxCompute Reader]()。本案例中的分区信息为`pt=1`。 **说明：** 如果待同步的数据量较大，可使用分区拆分数据进行同步。 |
    |`Writer`|用来配置Elasticsearch Writer，详情请参见[Elasticsearch Writer]()。     -   `index`：索引名称。
    -   `indexType`：索引类型，7.0及以上版本的Elasticsearch必须使用`_doc`。 |

6.  保存配置脚本，单击右侧的**调度配置**，按照需求配置相应的调度参数。

    各配置的详细说明请参见[调度配置]()章节。

    **说明：**

    -   在提交任务前，必须配置任务调度**依赖的上游节点**，详情请参见[依赖关系]()。
    -   如果您希望对任务进行周期性调度，需要配置任务的**时间属性**，包括任务的具体执行时间、调度周期、生效周期、重跑属性等。
    -   周期任务将于配置任务开始的第二天00:00，按照您的配置规则生效执行。
7.  配置执行同步任务所使用的资源组。

    ![选择资源组](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p69063.png)

    1.  在脚本配置页面右侧，单击**数据集成资源组配置**。

    2.  选择**方案**为**独享数据集成资源组**。

    3.  选择**独享数据集成资源组**为您创建的独享资源组。

8.  提交任务。

    1.  保存当前配置，单击![提交图标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p71328.png)图标。

    2.  在**提交新版本**对话框中，填入**备注**。

    3.  单击**确认**。

9.  单击![运行图标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p103889.png)图标，运行任务。

    任务运行过程中，可查看运行日志。运行成功后，显示如下结果。

    ![任务运行成功](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p103890.png)


## 步骤四：验证数据同步结果

1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    具体操作，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**（开发工具）。

3.  在**Console**中，执行如下命令查看同步的数据。

    ```
    POST /odps_index/_search?pretty
    {
    "query": { "match_all": {}}
    }
    ```

    **说明：** `odps_index`为您在数据同步脚本中设置的`index`字段的值。

    数据同步成功后，返回如下结果。

    ![查看同步的数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4034659951/p40128.png)

4.  执行如下命令，搜索文档中的`category`和`brand`字段。

    ```
    POST /odps_index/_search?pretty
    {
    "query": { "match_all": {} },
    "_source": ["category", "brand"]
    }
    ```

5.  执行如下命令，搜索`category`为`生鲜`的文档。

    ```
    POST /odps_index/_search?pretty
    {
    "query": { "match": {"category":"生鲜"} }
    }
    ```

6.  执行如下命令，按照`trans_num`字段对文档进行排序。

    ```
    POST /odps_index/_search?pretty
    {
    "query": { "match_all": {} },
    "sort": { "trans_num": { "order": "desc" } }
    }
    ```

    更多命令和访问方式，请参见[Elastic.co官方帮助中心](https://cloud.elastic.co/#help/)。


