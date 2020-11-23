---
keyword: synchronize MySQL data to Alibaba Cloud Elasticsearch
---

# Use DataWorks to synchronize MySQL data to Elasticsearch

Alibaba Cloud provides a variety of cloud storage and database services. To search for and analyze data stored in these services, you can use the Data Integration service provided by DataWorks to collect the offline data at a minimum interval of five minutes. Then, synchronize the data to Alibaba Cloud Elasticsearch. This topic uses ApsaraDB RDS for MySQL as an example.

## Overview

1.  [Preparations](#section_6j6_rzc_g3w)

    Prepare a MySQL data source, create a DataWorks workspace, and create and configure an Alibaba Cloud Elasticsearch cluster.

2.  [Step 1: Purchase and create an exclusive resource group](#section_3gy_ylz_9vs)

    Purchase and create an exclusive resource group for data integration. Bind the exclusive resource group to a virtual private cloud \(VPC\) and the created workspace. Exclusive resource groups transmit data in a fast and stable manner.

3.  [Step 2: Add data sources](#section_txr_ttb_6xt)

    Connect the MySQL data source and Elasticsearch cluster to the Data Integration service provided by DataWorks.

4.  [Step 3: Create and run a data synchronization task](#section_vhk_xd4_ndz)

    Configure a data synchronization script to import the data synchronized by Data Integration into the Elasticsearch cluster. The exclusive resource group is registered with Data Integration as a resource to run tasks. This resource group retrieves data from data sources and runs the task of writing data to the Elasticsearch cluster. The task is issued by Data Integration.

5.  [Step 4: Verify the data synchronization result](#section_ggi_pek_pgj)

    View the synchronized data in the Kibana console of your Elasticsearch cluster.


## Preparations

1.  Create a database.

    You can use an ApsaraDB RDS database or create a database on your on-premises machine. In this topic, an ApsaraDB RDS for MySQL database is used. Join two MySQL tables and then synchronize data to your Elasticsearch cluster. The following figures show the two MySQL tables. For more information, see [Create an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create an ApsaraDB RDS for MySQL instance.md).

    ![Table 1](../images/p40085.png "Table 1")

    ![Table 2](../images/p68992.png "Table 2")

2.  Create a DataWorks workspace.

    For more information, see [Create a workspace](). The workspace must reside in the same region as the ApsaraDB RDS for MySQL database.

3.  Create an Alibaba Cloud Elasticsearch cluster and enable the Auto Indexing feature for the cluster.

    The Elasticsearch cluster must reside in the same virtual private cloud \(VPC\) as the ApsaraDB RDS for MySQL database. For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md) and [Enable the Auto Indexing feature](/intl.en-US/Quick Start/Step 2: (Optional) Configure a cluster.md).


## Step 1: Purchase and create an exclusive resource group

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console).

2.  In the top navigation bar, select a region. In the left-side navigation pane, click **Resource Groups**.

3.  Purchase exclusive resources for data integration. For more information, see [Purchase an exclusive resource group for data integration]().

    **Note:** The exclusive resources for data integration must reside in the same region as the DataWorks workspace you created.

4.  Create an exclusive resource group for data integration. For more information, see [Add an exclusive resource group for data integration]().

    The following figure shows the configuration used in this example. **Resource Group Type** is set to **Exclusive Resource Groups for Data Integration**.

    ![Create an exclusive resource group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9630852061/p134902.png)

5.  Click **Add VPC Binding** on the right side of the created exclusive resource group to bind the exclusive resource group to a VPC. For more information, see [Bind an exclusive resource group for data integration to a VPC]().

    Exclusive resources are deployed in the VPC where DataWorks resides. DataWorks can be used to synchronize data from the ApsaraDB RDS for MySQL database to the Elasticsearch cluster only after it connects to the VPCs where the database and cluster reside. The ApsaraDB RDS for MySQL database and the Elasticsearch cluster reside in the same VPC. Therefore, when you bind the exclusive resource group to a VPC, you need only to select the **VPC** and **VSwitch** to which your Elasticsearch cluster belongs.

    ![Bind the exclusive resource group to a VPC](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9630852061/p134914.png)

6.  Click **Change Workspace** in the Actions column that corresponds to the exclusive resource group to bind it to the DataWorks workspace you created. For more information, see [Change the workspace to which an exclusive resource group is bound]().

    ![Bind an exclusive resource group to a workspace](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9630852061/p139131.png)


## Step 2: Add data sources

1.  Go to the **Data Integration** page.

    1.  In the left-side navigation pane of the DataWorks console, click **Workspaces**.

    2.  Find the workspace you created and click **Data Integration** in the **Actions** column.

2.  In the left-side navigation pane of the Data Integration page, click **Connection**.

3.  On the **Data Source** page, click **New data source** in the upper-right corner.

4.  In the **Add data source** dialog box, click **MySQL**. In the **Add MySQL data source** dialog box, configure the parameters.

    ![Parameter configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0457359951/p40090.png)

    **Connection Type**: **Alibaba Cloud instance mode** is used in this example. You can also select **Connection string mode**. For more information about the parameters, see [Configure a MySQL connection]().

    **Note:** If you select **Connection string mode**, you can configure **JDBC URL** by using the public endpoint of the ApsaraDB RDS for MySQL instance. However, you must add the elastic IP address \(EIP\) of the exclusive resource group to the whitelist of the ApsaraDB RDS for MySQL instance. For more information, see [Configure a whitelist for an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Control access to an ApsaraDB RDS for MySQL instance.md) and [Add the information about an exclusive resource group for Data Integration to the whitelist of a data store]().

    After parameters are configured, you can test the connectivity to the exclusive resource group. If the connectivity test is passed, **Connectable** appears in the **Connectivity status** column.

    ![Connectivity test succeeded](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9630852061/p139093.png)

5.  Click **Complete**.

6.  Add an Elasticsearch data source in the same way.

    ![Configuration of the Elasticsearch data source](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8502788951/p103880.png)

    |Parameter|Description|
    |---------|-----------|
    |**Endpoint**|The URL that is used to access the Elasticsearch cluster. Specify the URL in the following format: `http://<Internal or public endpoint of the Elasticsearch cluster>:9200`. You can obtain the endpoint from the Basic Information page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).**Note:** If you use the public endpoint of the cluster, add the elastic IP address \(EIP\) of the exclusive resource group to the public IP address whitelist of the cluster. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md) and [Add the information about an exclusive resource group for Data Integration to the whitelist of a data store](). |
    |**User name**|The username that is used to access the Elasticsearch cluster. The default username is elastic.|
    |**Password**|The password that is used to access the Elasticsearch cluster. The password of the elastic account is specified when you create the cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting a password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|

    **Note:** Configure the parameters that are not listed in the preceding table as required.


## Step 3: Create and run a data synchronization task

1.  On the Data Development tab of the DataWorks console, create a business process.

    For more information, see [Manage workflows]().

2.  In the navigation tree, right-click **Data Integration** and choose **New** \> **Offline synchronization**.

3.  In the **New node** dialog box, set **Node name** and click **Submit**.

4.  In the upper part of the page, click the ![Switch to the script mode](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0557359951/p69021.png) icon.

5.  Configure the data synchronization script.

    For more information, see [Create a sync node by using the code editor]().

    **Note:** You can click the ![Apply Template](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0557359951/p87612.png) icon in the upper part of the page to apply the required script configuration template. Then, modify the template as required.

    The following code provides a sample script used to retrieve information about students and their examination scores from two tables:

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
                        "id",
                        "name",
                        "sex",
                        "birth",
                        "department",
                        "address"
                    ],
                    "connection": [
                        {
                            "datasource": "zl_test_rdsmysql",
                            "querysql": [
                                "SELECT student.id,name,sex,birth,department,address,c_name,grade FROM student JOIN score on student.id=score.stu_id;"
                            ],
                            "table": [
                                "score"
                            ]
                        }
                    ],
                    "encoding": "UTF-8",
                    "splitPk": "",
                    "where": ""
                },
                "stepType": "mysql"
            },
            {
                "category": "writer",
                "name": "Writer",
                "parameter": {
                    "batchSize": 1000,
                    "cleanup": true,
                    "column": [
                        {
                            "name": "student_id",
                            "type": "id"
                        },
                        {
                            "name": "sex",
                            "type": "text"
                        },
                        {
                            "name": "name",
                            "type": "text"
                        },
                        {
                            "name": "birth",
                            "type": "integer"
                        },
                        {
                            "name": "quyu",
                            "type": "text"
                        },
                        {
                            "name": "address",
                            "type": "text"
                        },
                        {
                            "name": "cname",
                            "type": "text"
                        },
                        {
                            "name": "grades",
                            "type": "integer"
                        }
                    ],
                    "datasource": "ES_data_source",
                    "discovery": false,
                    "index": "mysqljoin",
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

    The synchronization script includes three parts.

    |Parameter|Description|
    |---------|-----------|
    |`setting`|Used to configure parameters related to packet loss and the maximum concurrency during synchronization. The default value of the `record` field in `errorLimit` is 0. You can change it to a larger value, such as 10.|
    |`Reader`|Used to configure the MySQL reader. Use `querysql` to define an SQL statement to retrieve data based on specific conditions. If `querysql` is configured, the MySQL reader ignores the `table`, `column`, `where`, and `splitPk` conditions. `datasource` uses `querysql` to parse the username and password. For more information, see [Configure MySQL Reader]().|
    |`Writer`|Used to configure the Elasticsearch writer. For more information, see [Elasticsearch Writer](). The configuration items include:     -   `index`: the name of the index.
    -   `indexType`: The index type of Elasticsearch clusters of V7.0 or later must be `_doc`. |

6.  Save the script. Then, in the right-side navigation pane, click the **Scheduling configuration** tab. In the pane that appears, configure parameters based on your business needs.

    For more information about the parameters, see [Scheduling configuration]().

    **Note:**

    -   Before you submit a task, you must set **Dependent upstream node** in the Scheduling dependency section of the Scheduling configuration pane. For more information, see [Dependencies]().
    -   If you want to schedule the task to run periodically, you must configure the parameters in the **Time attribute** section, such as Specific time, Scheduling cycle, Effective Date, and Rerun attribute.
    -   The configuration of a periodic task takes effect at 00:00 of the next day.
7.  Configure the resource group that is used to execute the synchronization task.

    ![Select a resource group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0557359951/p69063.png)

    1.  Click the **Data integration resource group configuration** tab on the right side of the page.

    2.  Set **Programme** to **Exclusive data integration Resource Group**.

    3.  Set **Exclusive data integration Resource Group** to the exclusive resource group you created.

8.  Submit the data synchronization task.

    1.  Save the current configurations and click the ![Submit icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8873433951/p71328.png) icon.

    2.  In the **Submit New Version** dialog box, enter your comments in the **Change description** field.

    3.  Click **OK**.

9.  Click the ![Run icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5104243061/p103889.png) icon to run the task.

    You can view the operational logs of the task when the task is running. After the task is successfully executed, the result shown in the following figure is returned.

    ![Task execution succeeded](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6104243061/p103890.png)


## Step 4: Verify the data synchronization result

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to query the synchronized data:

    ```
    POST /mysqljoin/_search? pretty
    {
    "query": { "match_all": {}}
    }
    ```

    **Note:** `mysqljoin` is the value that you configured for the `index` field in the data synchronization script.

    If the data is synchronized, the result shown in the following figure is returned.

    ![Query the synchronized data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0457359951/p40093.png)


