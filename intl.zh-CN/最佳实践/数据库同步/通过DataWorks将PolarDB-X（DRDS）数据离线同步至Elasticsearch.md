# 通过DataWorks将PolarDB-X（DRDS）数据离线同步至Elasticsearch

当您的业务数据存储在PolarDB-X（原DRDS升级版）中，需要进行全文检索和语义分析时，可借助阿里云Elasticsearch实现。本文介绍如何通过DataWorks，将PolarDB-X中的数据离线同步至Elasticsearch，并进行检索分析。

PolarDB-X是由阿里巴巴自主研发的云原生分布式数据库，融合分布式SQL引擎DRDS与分布式自研存储X-DB，基于云原生一体化架构设计，可支撑千万级并发规模及百PB级海量存储。专注解决海量数据存储、超高并发吞吐、大表瓶颈以及复杂计算效率等数据库瓶颈问题，历经各届天猫双十一及阿里云各行业客户业务的考验，助力企业加速完成业务数字化转型，详情请参见[PolarDB-X产品概述]()。

阿里云Elasticsearch兼容开源Elasticsearch的功能，以及Security、Machine Learning、Graph、APM等商业功能，致力于数据分析、数据搜索等场景服务。支持5.5.3、6.3.2、6.7.0、6.8.0、7.4.0和7.7.1等版本，并提供了商业插件X-Pack服务。在开源Elasticsearch的基础上提供企业级权限管控、安全监控告警、自动报表生成等功能，详情请参见[什么是阿里云Elasticsearch](/intl.zh-CN/产品简介/什么是阿里云Elasticsearch.md)。单击即可[免费试用](https://common-buy.aliyun.com/new?spm=a2c4g.11186623.2.9.ab722cbePLnzOs&commodityCode=elasticsearchpre&orderType=BUY&regionId=cn-hangzhou)。

## 操作流程

1.  [准备工作](#section_bxa_xqz_4sk)

    准备同一专有网络VPC（Virtual Private Cloud）下的阿里云PolarDB-X实例和Elasticsearch实例、在PolarDB-X实例中准备待同步的数据、开通DataWorks的数据集成和数据开发服务。

    **说明：** 建议您在同一VPC下进行数据同步，这样可以提高同步任务的稳定性。

2.  [步骤一：购买并创建独享资源组](#section_sxq_xgq_fhb)

    在DataWorks中，购买并创建独享资源组。为确保网络互通，您还需要为独享资源组绑定PolarDB-X实例所在的专有网络。

    **说明：** 独享资源组可以保障数据快速、稳定地传输。

3.  [步骤二：添加数据源](#section_5fi_9hu_d7k)

    在DataWorks中，创建DRDS和Elasticsearch数据源。

4.  [步骤三：配置并运行数据同步任务](#section_itx_whu_1el)

    在DataWorks中，使用向导模式配置数据同步任务，将**数据来源**指定为DRDS数据源，**数据去向**指定为Elasticsearch数据源，并配置字段映射关系。

5.  [步骤四：查看数据同步结果](#section_zfz_dem_rj0)

    在Elasticsearch实例的Kibana控制台中，查看同步成功的数据量，并对指定字段进行数据检索。


## 准备工作

1.  创建阿里云PolarDB-X 1.0实例、构建数据库和表，并插入数据。

    具体操作步骤请参见[SQL基本操作]()。本文使用的测试数据如下。

    ![测试数据](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5473343061/p134885.png)

    **说明：** 数据库创建完成后，默认允许所有IP地址访问。出于数据安全考虑，建议仅将待访问机器的IP地址加入白名单，详情请参见[设置白名单]()。

2.  创建DataWorks工作空间。

    具体操作步骤请参见[创建工作空间]()。创建时，所选地域需要与阿里云PolarDB-X实例所在地域保持一致。

3.  创建阿里云Elasticsearch实例，并开启自动创建索引功能。

    具体操作步骤请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)和[开启自动创建索引](/intl.zh-CN/快速入门/步骤二：配置实例（可选）.md)。创建实例时，所选专有网络和虚拟交换机需要与PolarDB-X实例保持一致。


## 步骤一：购买并创建独享资源组

1.  登录[DataWorks控制台](https://workbench.data.aliyun.com/console)。

2.  选择相应地域后，在左侧导航栏，单击**资源组列表**。

3.  参见[购买独享数据集成资源组]()，购买独享数据集成资源。

    **说明：** 购买时，所选地域需要与目标工作空间保持一致。

4.  参见[新增独享数据集成资源组]()，创建一个独享数据集成资源。

    本文使用的配置如下，其中**资源组类型**选择**独享数据集成资源组**。

    ![创建独享资源组](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3034659951/p134902.png)

5.  单击已创建的独享资源组右侧的**专有网络绑定**，参见[绑定专有网络]()，为该独享资源组绑定专有网络。

    独享资源部署在DataWorks托管的专有网络中，需要与PolarDB-X和Elasticsearch实例的专有网络连通才能同步数据，因此在绑定专有网络时，需要选择PolarDB-X实例所在**专有网络**和**交换机**。

    ![绑定专有网络](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p134914.png)

6.  单击已创建的独享资源组右侧的**修改归属工作空间**，参见[修改归属工作空间]()，为该独享资源组绑定目标工作空间。

    ![绑定工作空间](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4034659951/p139131.png)


## 步骤二：添加数据源

1.  进入DataWorks的**数据集成**页面。

    1.  在DataWorks控制台的左侧导航栏，单击**工作空间列表**。

    2.  找到目标工作空间，单击其右侧**操作**列下的**进入数据集成**。

2.  在左侧导航栏，单击**数据源**。

3.  在**数据源管理**页面，单击**新增数据源**。

4.  在**新增数据源**对话框中，单击**DRDS**。

5.  在**新增DRDS数据源**对话框中，填写数据源信息并测试连通性。连通成功后，单击**完成**。

    ![添加DRDS数据源](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8602659951/p134922.png)

    |参数|说明|
    |--|--|
    |**数据源类型**|本文使用**连接串模式**。您也可以选择**阿里云数据库（DRDS）**类型，详情请参见[配置DRDS数据源]()。|
    |**数据源名称**|必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**JDBC URL**|JDBC连接信息，格式为jdbc:mysql://ServerIP:Port/Database。此处需要将ServerIP:Port替换为PolarDB-X实例的VPC地址:VPC端口；Database替换为您已创建的PolarDB-X数据库名称。|
    |**用户名**|数据库对应的用户名。|
    |**密码**|数据库对应的密码。|

6.  使用同样的方式，添加Elasticsearch数据源。

    ![添加Elasticsearch数据源](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8602659951/p134923.png)

    |参数|说明|
    |--|--|
    |**数据源名称**|必须以字母、数字、下划线组合，且不能以数字和下划线开头。|
    |**数据源描述**|对数据源进行简单描述，不得超过80个字符。|
    |**Endpoint**|格式为http://<Elasticsearch实例的内网地址\>:9200。可参见[查看实例的基本信息](/intl.zh-CN/实例管理/管理实例/查看实例的基本信息.md)，获取Elasticsearch实例的内网地址。|
    |**用户名**|Elasticsearch实例的用户名，默认为**elastic**。|
    |**密码**|对应用户的密码。**elastic**用户的密码在创建实例时设定，如果忘记可进行重置，重置密码的注意事项和操作步骤请参见[重置实例访问密码](/intl.zh-CN/实例管理/安全配置/重置实例访问密码.md)。|


## 步骤三：配置并运行数据同步任务

1.  在DataWorks的数据开发页面，新建一个业务流程。

    具体操作步骤请参见[管理业务流程]()。

2.  展开新建的业务流程，右键单击**数据集成**，选择**新建** \> **离线同步**。

3.  在**新建节点**对话框中，输入**节点名称**，单击**提交**。

4.  在**选择数据源**区域中，将**数据来源**指定为DRDS数据源，并填入待同步的表名称；将**数据去向**指定为Elasticsearch数据源，并填入索引名和索引类型。

    ![选择数据源](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9602659951/p134948.png)

    **说明：**

    -   您也可以使用脚本模式配置数据同步，详情请参见[通过脚本模式配置任务]()、[DRDS Reader]()和[Elasticsearch Writer]()。
    -   建议在**Elasticsearch**数据源的**高级配置**下，将**启用节点发现**设置为**否**，否则同步过程中提示连接超时。
5.  在**字段映射**区域中，设置**来源字段**与**目标字段**的映射关系。

    本示例中，**来源字段**保持默认，仅修改**目标字段**。在**目标字段**右侧，单击![修改字段图标](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9602659951/p134953.png)图标，在对话框中输入如下字段配置。

    ```
    {"name":"Name","type":"text"}
    {"name":"Platform","type":"text"}
    {"name":"Year_of_Release","type":"date"}
    {"name":"Genre","type":"text"}
    {"name":"Publisher","type":"text"}
    {"name":"na_Sales","type":"float"}
    {"name":"EU_Sales","type":"float"}
    {"name":"JP_Sales","type":"float"}
    {"name":"Other_Sales","type":"float"}
    {"name":"Global_Sales","type":"float"}
    {"name":"Critic_Score","type":"long"}
    {"name":"Critic_Count","type":"long"}
    {"name":"User_Score","type":"float"}
    {"name":"User_Count","type":"long"}
    {"name":"Developer","type":"text"}
    {"name":"Rating","type":"text"}
    ```

    配置完成后，效果如下。

    ![配置字段映射](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9602659951/p134955.png)

6.  在**通道控制**区域中，配置执行任务的相关参数。

    本示例使用默认配置。

    ![通道控制](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9602659951/p134957.png)

    **说明：** 如果您需要定时调度任务，可参见[调度配置]()章节进行配置。

7.  在页面上方，单击![运行图标](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9602659951/p134958.png)图标，查看**运行日志**。

    日志输出**successfully**表示任务运行成功；输出**FINISH**表示任务运行完成。

    ![运行任务](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9602659951/p134965.png)


## 步骤四：查看数据同步结果

1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    具体操作步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**。

3.  在**Console**中，执行如下命令，查询目标端数据量。

    **说明：** 您可以将目标端数据量与源端数据量进行对比，确认数据是否全部同步成功。

    ```
    GET drdstest/_search
    {
      "query": {
        "match_all": {}
      }
    }
    ```

    执行成功后，结果如下。

    ![查看目标端数据量](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9602659951/p134970.png)

4.  执行如下命令，对指定字段进行数据检索。

    ```
    GET drdstest/_search
    {
      "query": {
        "term": {
          "Publisher.keyword": {
            "value": "Nintendo"
          }
        }
      }
    }
    ```

    执行成功后，返回如下结果。

    ![对字段进行数据检索](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9602659951/p134972.png)


