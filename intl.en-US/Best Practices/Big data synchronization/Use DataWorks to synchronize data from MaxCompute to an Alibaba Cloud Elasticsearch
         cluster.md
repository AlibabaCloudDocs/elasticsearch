---
keyword: [synchronize data from ODPS to Alibaba Cloud Elasticsearch, synchronize data from MaxCompute to Alibaba Cloud Elasticsearch]
---

# Use DataWorks to synchronize data from MaxCompute to an Alibaba Cloud Elasticsearch cluster

Alibaba Cloud provides a variety of cloud storage and database services. To search for and analyze the data stored in these services, you can use the Data Integration feature of DataWorks to collect offline data at intervals of at least five minutes. Then, synchronize the data to an Alibaba Cloud Elasticsearch cluster. This topic describes how to synchronize data from MaxCompute to an Alibaba Cloud Elasticsearch cluster.

Alibaba Cloud Elasticsearch supports the following offline data sources:

-   Alibaba Cloud databases: ApsaraDB RDS for MySQL, ApsaraDB RDS for PostgreSQL, ApsaraDB RDS for SQL Server, ApsaraDB RDS for PPAS, ApsaraDB for MongoDB, and ApsaraDB for HBase
-   Distributed Relational Database Service \(DRDS\)
-   MaxCompute
-   Object Storage Service \(OSS\)
-   Tablestore
-   User-created databases: HDFS, Oracle, FTP, DB2, MySQL, PostgreSQL, SQL Server, PPAS, MongoDB, and HBase

## Procedure

1.  [Preparations](#section_cx6_k3k_xsa)

    Create a DataWorks workspace, activate MaxCompute, prepare the MaxCompute data source, and create an Alibaba Cloud Elasticsearch cluster.

2.  [Step 1: Purchase and create an exclusive resource group](#section_5u1_9vc_phf)

    Purchase and create an exclusive resource group for data integration. Bind the exclusive resource group to a virtual private cloud \(VPC\) and the created workspace. Exclusive resource groups transmit data in a fast and stable manner.

3.  [Step 2: Add data sources](#section_enz_k2e_5on)

    Connect MaxCompute and Elasticsearch data sources to Data Integration.

4.  [Step 3: Create and run a data synchronization task](#section_qr9_459_75g)

    Configure a data synchronization script to import the data synchronized by Data Integration into the Elasticsearch cluster. The exclusive resource group is registered with Data Integration as a resource to run tasks. This resource group retrieves data from data sources and runs the task of writing data to the Elasticsearch cluster. The task is issued by Data Integration.

5.  [Step 4: View synchronization results](#section_v84_pvy_wz7)

    In the Kibana console, view the synchronized data and search for data based on specific conditions.


## Preparations

1.  Create a DataWorks workspace. Select MaxCompute as the computing engine.

    For more information, see [Create a project](/intl.en-US/Prepare/Create a project.md).

2.  Create a table in MaxCompute and import data into the table.

    For more information, see [Create and view a table](/intl.en-US/Quick Start/Create and view a table.md) and [Import data](/intl.en-US/Quick Start/Import data.md).

    The following figures show the table schema and a part of the table data.

    ![Table schema](../images/p40112.png "Table schema")

    ![Table data](../images/p40113.png "Table data")

    **Note:** The provided data is only for tests. You can migrate data from Hadoop to MaxCompute and synchronize the data to your Elasticsearch cluster. For more information, see [Best practices of migrating data from Hadoop to MaxCompute](/intl.en-US/Best Practices/Data migration/Best practices of migrating data from Hadoop to MaxCompute.md).

3.  Create an Elasticsearch cluster and enable the Auto Indexing feature for the cluster.

    For more information, see [Create an Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Elasticsearch cluster.md) and [Enable auto indexing](/intl.en-US/Quick Start/Step 2 (optional): Configure a cluster.md). The Elasticsearch cluster must reside in the same region as the DataWorks workspace you created.


## Step 1: Purchase and create an exclusive resource group

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console).

2.  In the top navigation bar, select a region. In the left-side navigation pane, click **Resource Groups**.

3.  Purchase exclusive resources for data integration. For more information, see [Purchase an exclusive resource group for data integration]().

    **Note:** The exclusive resources for data integration must reside in the same region as the DataWorks workspace you created.

4.  Create an exclusive resource group for data integration. For more information, see [Add an exclusive resource group for data integration]().

    The following figure shows the configuration used in this example. **Resource Group Type** is set to **Exclusive Resource Groups for Data Integration**.

    ![Create an exclusive resource group](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9630852061/p134902.png)

5.  Find the created exclusive resource group and click **Add VPC Binding** in the Actions column to bind the exclusive resource group to a VPC. For more information, see [Bind an exclusive resource group for data integration to a VPC]().

    Exclusive resources are deployed in VPCs managed by DataWorks. DataWorks can synchronize data to an Elasticsearch cluster only after DataWorks and the cluster are connected to the same VPC. Therefore, when you bind the exclusive resource group to a VPC, you must select the **VPC** and **VSwitch** to which your Elasticsearch cluster belongs.

    ![Bind an exclusive resource group for data integration to a VPC](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9630852061/p134914.png)

6.  Click **Change Workspace** in the Actions column that corresponds to the exclusive resource group to bind it to the DataWorks workspace you created. For more information, see [Change the workspace to which an exclusive resource group is bound]().

    ![Bind an exclusive resource group to a workspace](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9630852061/p139131.png)


## Step 2: Add data sources

1.  Go to the **Data Integration** page.

    1.  In the left-side navigation pane of the DataWorks console, click **Workspaces**.

    2.  Find the workspace you created and click **Data Integration** in the **Actions** column.

2.  In the left-side navigation pane of the Data Integration page, click **Connection**.

3.  In the left-side navigation pane of the page that appears, click **Data Source**. Then, click **New data source** in the upper-right corner.

4.  In the Big Data Storage section of the **Add data source** dialog box, click **MaxCompute \(ODPS\)**. In the **Add MaxCompute \(ODPS\) data source** dialog box, configure the parameters.

    ![Add the MaxCompute (ODPS) data source](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0557359951/p40122.png)

    |Parameter|Description|
    |---------|-----------|
    |**ODPS Endpoint**|The endpoint of MaxCompute, which varies in different regions. For more information, see [Configure endpoints](/intl.en-US/Prepare/Configure endpoints.md).|
    |**ODPS project name**|To obtain the project name, log on to the [DataWorks console](https://workbench.data.aliyun.com/console). In the left-side navigation pane, click **MaxCompute** below **Compute Engines**.|
    |**AccessKey ID**|To obtain the AccessKey ID, move the pointer over your profile picture and click **AccessKey Management**.|
    |**AccessKey Secret**|To obtain the AccessKey secret, move the pointer over your profile picture and click **AccessKey Management**.|

    **Note:** Configure the parameters that are not listed in the preceding table as required or use their default values.

    After parameters are configured, you can test the connectivity to the exclusive resource group. If the connectivity test is passed, **Connectable** appears in the **Connectivity status** column.

    ![Connectivity test succeeded](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9630852061/p139093.png)

5.  Click **Complete**.

6.  Add an Elasticsearch data source in the same way.

    ![Configuration of the Elasticsearch data source](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8502788951/p103880.png)

    |Parameter|Description|
    |---------|-----------|
    |**Endpoint**|The URL that is used to access the Elasticsearch cluster. Specify the URL in the following format: `http://<Internal or public endpoint of the Elasticsearch cluster>:9200`. You can obtain the endpoint from the Basic Information page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).**Note:** If you use the public endpoint of the cluster, add the elastic IP address \(EIP\) of the exclusive resource group to the public IP address whitelist of the cluster. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md) and [Add the information about an exclusive resource group for Data Integration to the whitelist of a data store](). |
    |**User name**|The username that is used to access the Elasticsearch cluster. The default username is elastic.|
    |**Password**|The password that is used to access the Elasticsearch cluster. The password of the elastic account is specified when you create the cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting a password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|

    **Note:** Configure the parameters that are not listed in the preceding table as required.


## Step 3: Create and run a data synchronization task

1.  On the Data Development tab of the DataWorks console, create a business process.

    For more information, see [t1693630.md\#]().

2.  In the navigation tree, right-click **Data Integration** and choose **New** \> **Offline synchronization**.

3.  In the **New node** dialog box, set **Node name** and click **Submit**.

4.  In the upper part of the page, click the ![Switch to the script mode](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0557359951/p69021.png) icon.

5.  In the Tips message, click OK. Then, configure the data synchronization script.

    For more information, see [Create a sync node by using the code editor]().

    **Note:** You can also click the ![Import Template](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0557359951/p87612.png) icon in the upper part of the page to import a script configuration template. Then, modify the template as required.

    The following code provides a sample script:

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
                    "datasource": "es_test",
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

    The preceding script includes three parts.

    |Part|Description|
    |----|-----------|
    |`setting`|Used to configure parameters related to packet loss and the maximum concurrency during synchronization. **Note:** If the volume of data you want to synchronize is large, you can increase the maximum concurrency. |
    |`Reader`|Used to configure MaxCompute as the reader. If your MaxCompute table is a partitioned table, you must configure partition information by using the `partition` field. For more information, see [Configure MaxCompute Reader](). The partition information in this example is `pt=1`. **Note:** If the volume of data you want to synchronize is large, you can split the data into partitions and synchronize the data by partition. |
    |`Writer`|Used to configure the Elasticsearch cluster as the writer. For more information, see [Configure Elasticsearch Writer]().     -   `index`: the name of the destination index.
    -   `indexType`: the type of the destination index. The index type of Elasticsearch clusters of V7.0 or later must be `_doc`. |

6.  Save the script. Then, in the right-side navigation pane, click the **Scheduling configuration** tab. In the pane that appears, configure parameters based on your business needs.

    For more information about the parameters, see [Scheduling configuration]().

    **Note:**

    -   Before you submit a task, you must set **Dependent upstream node** in the Scheduling dependency section of the Scheduling configuration pane. For more information, see [Dependencies]().
    -   If you want to schedule the task to run periodically, you must configure the parameters in the **Time attribute** section, such as Specific time, Scheduling cycle, Effective Date, and Rerun attribute.
    -   The configuration of a periodic task takes effect at 00:00 of the next day.
7.  Configure the resource group that is used to execute the synchronization task.

    ![Select a resource group](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0557359951/p69063.png)

    1.  Click the **Data integration resource group configuration** tab on the right side of the page.

    2.  Set **Programme** to **Exclusive data integration Resource Group**.

    3.  Set **Exclusive data integration Resource Group** to the exclusive resource group you created.

8.  Submit the data synchronization task.

    1.  Save the current configurations and click the ![Submit icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8873433951/p71328.png) icon.

    2.  In the **Submit New Version** dialog box, enter your comments in the **Change description** field.

    3.  Click **OK**.

9.  Click the ![Run icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5104243061/p103889.png) icon to run the task.

    You can view the operational logs of the task when the task is running. After the task is successfully executed, the result shown in the following figure is returned.

    ![Task execution succeeded](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6104243061/p103890.png)


## Step 4: View synchronization results

1.  Log on to the Kibana console of the Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to query the synchronized data:

    ```
    POST /odps_index/_search?pretty
    {
    "query": { "match_all": {}}
    }
    ```

    **Note:** `odps_index` is the value that you specified for the `index` field in the data synchronization script.

    If the data is synchronized, the result shown in the following figure is returned.

    ![Query the synchronized data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0730852061/p40128.png)

4.  Run the following command to query the `category` and `brand` fields in the data:

    ```
    POST /odps_index/_search?pretty
    {
    "query": { "match_all": {} },
    "_source": ["category", "brand"]
    }
    ```

5.  Run the following command to query data entries where the value of the `category` field is `fresh`:

    ```
    POST /odps_index/_search?pretty
    {
    "query": { "match": {"category":"fresh"} }
    }
    ```

6.  Run the following command to sort the data based on the `trans_num` field:

    ```
    POST /odps_index/_search?pretty
    {
    "query": { "match_all": {} },
    "sort": { "trans_num": { "order": "desc" } }
    }
    ```

    For more information, see [open source Elastic documentation](https://cloud.elastic.co/#help/).


