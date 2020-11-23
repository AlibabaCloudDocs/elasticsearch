# Use DataWorks to synchronize data from a Hadoop cluster to an Alibaba Cloud Elasticsearch cluster

When you use a Hadoop cluster to perform interactive big data analytics and queries, the process may be time-consuming. To address this issue, you can synchronize data from the Hadoop cluster to an Alibaba Cloud Elasticsearch cluster for analytics and queries. Elasticsearch can respond to multiple types of queries within seconds, especially ad hoc queries. This topic describes how to synchronize data from a Hadoop cluster to an Elasticsearch cluster by using the data synchronization feature of DataWorks.

## Procedure

1.  [Preparations](#section_yzz_in4_vmt)

    Create a Hadoop cluster, a DataWorks workspace, and an Elasticsearch cluster. Configure the Elasticsearch cluster.

2.  [Step 1: Prepare data](#section_y14_epz_42i)

    Create test data in the Hadoop cluster.

3.  [Step 2: Purchase and create an exclusive resource group](#section_j0f_5be_uzv)

    Purchase and create an exclusive resource group for data integration. Bind the exclusive resource group to a virtual private cloud \(VPC\) and the created workspace. Exclusive resource groups transmit data in a fast and stable manner.

4.  [Step 3: Add data sources](#section_rh6_39y_kw4)

    Connect the Elasticsearch cluster and the HDFS of the Hadoop cluster to the Data Integration service of DataWorks.

5.  [Step 4: Create and run a data synchronization task](#section_p0a_vwy_icq)

    Configure a data synchronization script to store the data collected by Data Integration to the Elasticsearch cluster. The exclusive resource group is registered with Data Integration as a resource to run tasks. This resource group retrieves data from data sources and runs the task of writing data to the Elasticsearch cluster. The task is issued by Data Integration.

6.  [Step 5: View synchronization results](#section_ti9_ca0_mkb)

    In the Kibana console, view the synchronized data and search for data based on specific conditions.


## Preparations

1.  Create a Hadoop cluster.

    Before you synchronize data, make sure that your Hadoop cluster runs normally. In this step, the Alibaba Cloud E-MapReduce \(EMR\) service is used to automatically create a Hadoop cluster. For more information, see [Create a cluster](/intl.en-US/Quick Start/Create a cluster.md).

    Sample configurations of the EMR Hadoop cluster: \(Default configurations are used for items that are not listed. You can also modify the default configurations based on your business needs.\)

    -   **Cluster Type**: Hadoop
    -   **EMR Version**: EMR-3.26.3
    -   **Assign Public IP Address**: turned on
2.  Create an Elasticsearch cluster and enable the Auto Indexing feature for the cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md) and [Enable the Auto Indexing feature](/intl.en-US/Quick Start/Step 2: (Optional) Configure a cluster.md). Make sure that the Elasticsearch cluster resides in the same virtual private cloud \(VPC\), region, and zone as the EMR Hadoop cluster. In this step, an Elasticsearch V6.7.0 cluster of the Standard Edition is created.

3.  Create a DataWorks workspace.

    Make sure that the workspace resides in the same region as the Elasticsearch cluster. For more information, see [Create a workspace]().


## Step 1: Prepare data

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your EMR Hadoop cluster resides.

3.  In the **Clusters** section of the page that appears, find your EMR Hadoop cluster and click its ID.

4.  In the upper part of the page, click the **Data Platform** tab.

5.  Click **Create Project** to create a data development project. In this step, set Select Resource Group to Default Resource Group.

    For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).

6.  In the **Projects** section, find the created project and click **Edit Job** in the **Actions** column to create a job.

    For more information, see [Edit jobs](/intl.en-US/Data Development/Edit jobs.md). In this step, set Job Type to Hive.

7.  Create a data table and insert data into the table.

    1.  In the code editor, enter a statement to create a Hive table. Then, click **Run**.

        In this step, the following statement is used:

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

    2.  In the **Run Job** dialog box, configure the parameters and click **OK**.

        -   Set Select Resource Group to Default Resource Group.
        -   Set Target Cluster to the cluster you created.
    3.  Create another job. In the code editor, enter the following SQL statement to insert test data.

        You can import data from Object Storage Service \(OSS\) or other data sources. You can also manually insert data. In this step, data is manually inserted.

        ```
        insert into
        hive_esdoc_good_sale PARTITION(pt =1 ) values('2018-08-21','Coat','Brand A','lilei',3,500.6,7),('2018-08-22','Fresh','Brand B','lilei',1,303,8),('2018-08-22','Coat','Brand C','hanmeimei',2,510,2),(2018-08-22,'Bathroom','Brand A','hanmeimei',1,442.5,1),('2018-08-22','Fresh','Brand D','hanmeimei',2,234,3),('2018-08-23','Coat','Brand B','jimmy',9,2000,7),('2018-08-23','Fresh','Brand A','jimmy',5,45.1,5),('2018-08-23','Coat','Brand E','jimmy',5,100.2,4),('2018-08-24','Fresh','Brand G','peiqi',10,5560,7),('2018-08-24','Bathroom','Brand F','peiqi',1,445.6,2),('2018-08-24','Coat','Brand A','ray',3,777,3),('2018-08-24','Bathroom','Brand G','ray',3,122,3),('2018-08-24','Coat','Brand C','ray',1,62,7) ;
        ```

8.  Check whether the data is inserted.

    1.  Create a job for an ad hoc query.

        For more information, see [Implement ad hoc queries](/intl.en-US/Data Development/Implement ad hoc queries.md).

    2.  Enter the following SQL statement and click **Run**:

        ```
        select * from hive_esdoc_good_sale where pt =1;
        ```

    3.  In the lower part of the page, click the **Records** tab. On this tab, click **Details** in the **Action** column.

    4.  In the upper part of the page, click the **Scheduling Center** tab. On this tab, click the **Execution Result** tab.

        Then, you can check whether the data is inserted into the Hive table of the Hadoop cluster for synchronization. The following figure shows the inserted data.

        ![View test data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8502788951/p103731.png)


## Step 2: Purchase and create an exclusive resource group

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console).

2.  In the top navigation bar, select a region. In the left-side navigation pane, click **Resource Groups**.

3.  Purchase exclusive resources for data integration. For more information, see [Purchase an exclusive resource group for data integration]().

    **Note:** The exclusive resources for data integration must reside in the same region as the DataWorks workspace you created.

4.  Create an exclusive resource group for data integration. For more information, see [Add an exclusive resource group for data integration]().

    The following figure shows the configuration used in this example. **Resource Group Type** is set to **Exclusive Resource Groups for Data Integration**.

    ![Create an exclusive resource group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9630852061/p134902.png)

5.  Find the created exclusive resource group. Then, click **Add VPC Binding** in the Actions column to bind the exclusive resource group to a VPC. For more information, see [Bind an exclusive resource group for data integration to a VPC]().

    Exclusive resources are deployed in a VPC managed by DataWorks. DataWorks can be used to synchronize data from the Hadoop cluster to the Elasticsearch cluster only after it connects to the VPCs where the clusters reside. In this topic, the Hadoop cluster and Elasticsearch cluster reside in the same VPC. Therefore, you only need to select the **VPC** and **VSwitch** of the Elasticsearch cluster for the binding.

    ![Bind an exclusive resource group for data integration to a VPC](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9630852061/p134914.png)

6.  Click **Change Workspace** in the Actions column that corresponds to the exclusive resource group to bind it to the DataWorks workspace you created. For more information, see [Change the workspace to which an exclusive resource group is bound]().

    ![Bind an exclusive resource group to a workspace](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9630852061/p139131.png)


## Step 3: Add data sources

1.  Go to the **Data Integration** page.

    1.  In the left-side navigation pane of the DataWorks console, click **Workspaces**.

    2.  Find the workspace you created and click **Data Integration** in the **Actions** column.

2.  In the left-side navigation pane of the Data Integration page, click **Connection**.

3.  On the **Data Source** page, click **New data source** in the upper-right corner.

4.  In the **Semi-structured storage** section of the **Add data source** dialog box, click **HDFS**.

5.  In the **Add HDFS data source** dialog box, specify **Data Source Name** and **DefaultFS**.

    ![Add an HDFS data source](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8502788951/p103833.png)

    **DefaultFS**: If your EMR Hadoop cluster is in non-HA mode, set this parameter to `hdfs://Internal IP address of emr-header-1:9000`. If your EMR Hadoop cluster is in HA mode, set this parameter to `hdfs://Internal IP address of emr-header-1:8020`. The internal IP address of emr-header-1 is used because emr-header-1 communicates with DataWorks over a VPC.

    After the parameters are configured, you can test the connectivity to the exclusive resource group. If the connectivity test is passed, **Connectable** appears in the **Connectivity status** column.

    ![Connectivity test succeeded](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9630852061/p139093.png)

6.  Click **Complete**.

7.  Add an Elasticsearch data source in the same way.

    ![Configuration of the Elasticsearch data source](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8502788951/p103880.png)

    |Parameter|Description|
    |---------|-----------|
    |**Endpoint**|The URL that is used to access the Elasticsearch cluster. Specify the URL in the following format: `http://<Internal or public endpoint of the Elasticsearch cluster>:9200`. You can obtain the endpoint from the Basic Information page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).**Note:** If you use the public endpoint of the cluster, add the elastic IP address \(EIP\) of the exclusive resource group to the public IP address whitelist of the cluster. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md) and [Add the information about an exclusive resource group for Data Integration to the whitelist of a data store](). |
    |**User name**|The username that is used to access the Elasticsearch cluster. The default username is elastic.|
    |**Password**|The password that is used to access the Elasticsearch cluster. The password of the elastic account is specified when you create the cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|

    **Note:** Configure the parameters that are not listed in the preceding table as required.


## Step 4: Create and run a data synchronization task

1.  On the Data Development tab of the DataWorks console, create a business process.

    For more information, see [Manage workflows]().

2.  In the navigation tree, right-click **Data Integration** and choose **New** \> **Offline synchronization**.

3.  In the **New node** dialog box, set **Node name** and click **Submit**.

4.  In the upper part of the page, click the ![Switch to the script mode](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0557359951/p69021.png) icon.

5.  In the Tips message, click OK. Then, configure the data synchronization script.

    For more information, see [Create a sync node by using the code editor]().

    **Note:** You can also click the ![Import Template](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0557359951/p87612.png) icon in the upper part of the page to import a script configuration template. Then, modify the template as required.

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

    The preceding script includes three parts.

    |Part|Description|
    |----|-----------|
    |`setting`|Used to configure parameters related to packet loss and the maximum concurrency during synchronization. The default value of the `record` field in the `errorLimit` parameter is 0. You must set the field to a larger value, such as 10.|
    |`Reader`|Used to configure the Hadoop cluster as the reader. `path` specifies the location of the data that is stored in the Hadoop cluster. To obtain the location, log on to the master node of the Hadoop cluster and run the `hdfs dfs –ls /user/hive/warehouse/hive_esdoc_good_sale` command. For a partitioned table, the data synchronization feature of DataWorks can automatically recurse to the partition where the data is stored. For more information, see [Configure HDFS Reader]().|
    |`Writer`|Used to configure the Elasticsearch cluster as the writer. For more information, see [Elasticsearch Writer]().     -   `index`: the name of the destination index.
    -   `indexType`: the type of the destination index. The index type of Elasticsearch clusters of V7.0 or later must be `_doc`. |

6.  Save the script. Then, in the right-side navigation pane, click the **Scheduling configuration** tab. In the pane that appears, configure parameters based on your business needs.

    For more information about the parameters, see [Scheduling configuration]().

    **Note:**

    -   Before you submit a task, you must set **Dependent upstream node** in the Scheduling dependency section of the Scheduling configuration pane. For more information, see [Dependencies]().
    -   If you want to schedule the task to run periodically, you must configure the parameters in the **Time attribute** section, such as Specific time, Scheduling cycle, Effective Date, and Rerun attribute.
    -   The configuration of a periodic task takes effect at 00:00 of the next day.
7.  Configure the resource group that is used to execute the synchronization task.

    ![Select a resource group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0557359951/p69063.png)

    1.  In the right-side navigation pane, click the **Data integration resource group configuration** tab.

    2.  Set **Programme** to **Exclusive data integration Resource Group**.

    3.  Set **Exclusive data integration Resource Group** to the exclusive resource group you created.

8.  Submit the data synchronization task.

    1.  Save the current configurations and click the ![Submit icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8873433951/p71328.png) icon.

    2.  In the **Submit New Version** dialog box, enter your comments in the **Change description** field.

    3.  Click **OK**.

9.  Click the ![Run icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5104243061/p103889.png) icon to run the task.

    You can view the operational logs of the task when the task is running. After the task is successfully executed, the result shown in the following figure is returned.

    ![Task execution succeeded](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6104243061/p103890.png)


## Step 5: View synchronization results

1.  Log on to the Kibana console of the destination Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to query the synchronized data:

    ```
    POST /hive_esdoc_good_sale/_search?pretty
    {
    "query": { "match_all": {}}
    }
    ```

    **Note:** `hive_esdoc_good_sale` is the index name that is specified by the `index` field in the data synchronization script.

    If the data is synchronized, the result shown in the following figure is returned.

    ![View synchronized data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7615243061/p40081.png)

4.  Run the following command to search for all documents that contain Brand A:

    ```
    POST /hive_esdoc_good_sale/_search?pretty
    {
      "query": { "match_phrase": { "brand":"Brand A" } }
    }
    ```

    ![All documents that contain Brand A returned](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7615243061/p40082.png)

5.  Run the following command to sort products of each brand based on the **number of clicks**. Then, determine the popularity of the products:

    ```
    POST /hive_esdoc_good_sale/_search?pretty
    {
    "query": { "match_all": {} },
    "sort": { "click_cnt": { "order": "desc" } },
    "_source": ["category", "brand","click_cnt"]
    }
    ```

    ![Products sorted by the number of clicks](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7615243061/p40083.png)

    For more information about other commands and their use scenarios, see [Alibaba Cloud Elasticsearch documentation](https://www.alibabacloud.com/help/product/57736.htm) and [open source Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/index.html).


