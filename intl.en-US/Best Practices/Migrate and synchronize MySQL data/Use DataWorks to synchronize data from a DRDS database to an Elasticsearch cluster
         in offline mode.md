# Use DataWorks to synchronize data from a DRDS database to an Elasticsearch cluster in offline mode

Your business data is stored in a Distributed Relational Database Service \(DRDS\) database. If you want to perform full-text searches and semantic analytics on the data, you can synchronize the data to an Alibaba Cloud Elasticsearch cluster in offline mode.

DRDS is developed by Alibaba Cloud. It integrates the distributed SQL engine DRDS and the proprietary distributed storage X-DB. Based on the integrated cloud-native architecture, it supports tens of millions of concurrent connections and can store hundreds of petabytes of data. It aims to provide solutions for massive data storage, ultra-high concurrent throughput, large table performance bottlenecks, and complex computing efficiency. DRDS has become a mature service after it is applied to Double 11 and the business of Alibaba Cloud customers in various industries. The application of DRDS boosts the digital transformation of enterprises. For more information, see [DRDS overview]().

Alibaba Cloud Elasticsearch is compatible with open source Elasticsearch features, such as Security, Machine Learning, Graph, and Application Performance Management \(APM\). It is released in 5.5.3, 6.3.2, 6.7.0, 6.8.0, 7.4.0, and 7.7.1 versions. It supports the commercial plug-in X-Pack and is ideal for scenarios such as data analytics and searches. Based on open source Elasticsearch, Alibaba Cloud Elasticsearch implements enterprise-grade access control, security monitoring and alerting, and automated reporting. For more information, see [What is Alibaba Cloud Elasticsearch?](/intl.en-US/Product Introduction/What is Alibaba Cloud Elasticsearch?.md). You can click [here](https://common-buy.aliyun.com/new?spm=a2c4g.11186623.2.9.ab722cbePLnzOs&commodityCode=elasticsearchpre&orderType=BUY&regionId=cn-hangzhou) to apply for a free trial.

## Procedure

1.  [Preparations](#section_bxa_xqz_4sk)

    Create a DRDS instance and an Alibaba Cloud Elasticsearch cluster in the same virtual private cloud \(VPC\). Prepare the data that you want to migrate in the DRDS instance. Activate the Data Integration and Data Analytics services of DataWorks.

    **Note:** To improve the stability of synchronization tasks, we recommend that you synchronize data within a VPC.

2.  [Step 1: Purchase and create an exclusive resource group](#section_sxq_xgq_fhb)

    In the DataWorks console, purchase and create an exclusive resource group. To ensure the network connection, you must bind the exclusive resource group to the VPC where the DRDS instance resides.

    **Note:** Exclusive resource groups transmit data in a fast and stable manner.

3.  [Step 2: Add data sources](#section_5fi_9hu_d7k)

    In the DataWorks console, add the DRDS instance and the Elasticsearch cluster as data sources.

4.  [Step 3: Create and run a data synchronization task](#section_itx_whu_1el)

    In the DataWorks console, use the wizard mode to configure a data synchronization task. During the configuration, specify the DRDS instance as the **source connection** and the Elasticsearch cluster as the **destination connection**, and configure field mappings.

5.  [Step 4: View synchronization results](#section_zfz_dem_rj0)

    In the Kibana console of the Elasticsearch cluster, view the synchronized data and search for data by using a specific field.


## Preparations

1.  Create a DRDS V1.0 instance, a DRDS database, and a table. Then, insert data into the table.

    For more information, see [Basic SQL operations](). The following figure shows the test data that is used in this topic.

    ![Test data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9573343061/p134885.png)

    **Note:** After a database is created, all IP addresses are allowed to access the database by default. For security purposes, we recommend that you add only the IP address of the host that you use to the whitelist of the database. For more information, see [Set an IP address whitelist]().

2.  Create a DataWorks workspace.

    For more information, see [Create a workspace](). The workspace must reside in the same region as the DRDS instance you created.

3.  Create an Elasticsearch cluster and enable the Auto Indexing feature for the cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md) and [Enable the Auto Indexing feature](/intl.en-US/Quick Start/Step 2: (Optional) Configure a cluster.md). The cluster must belong to the same VPC and VSwitch as the DRDS instance.


## Step 1: Purchase and create an exclusive resource group

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console).

2.  In the top navigation bar, select a region. In the left-side navigation pane, click **Resource Groups**.

3.  Purchase exclusive resources for data integration. For more information, see [Purchase an exclusive resource group for data integration]().

    **Note:** The exclusive resources for data integration must reside in the same region as the DataWorks workspace you created.

4.  Create an exclusive resource group for data integration. For more information, see [Add an exclusive resource group for data integration]().

    The following figure shows the configuration used in this example. **Resource Group Type** is set to **Exclusive Resource Groups for Data Integration**.

    ![Create an exclusive resource group](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9630852061/p134902.png)

5.  Find the created exclusive resource group and click **Add VPC Binding** in the Actions column to bind the exclusive resource group to a VPC. For more information, see [Bind an exclusive resource group for data integration to a VPC]().

    Exclusive resources are deployed in VPCs managed by DataWorks. DataWorks can be used to synchronize data from a DRDS database to an Elasticsearch cluster only after it connects to the VPCs where the database and cluster reside. In this topic, the database and cluster reside in the same VPC. Therefore, you only need to select the **VPC** and **VSwitch** of the DRDS database for the binding.

    ![Bind an exclusive resource group for data integration to a VPC](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9630852061/p134914.png)

6.  Click **Change Workspace** in the Actions column that corresponds to the exclusive resource group to bind it to the DataWorks workspace you created. For more information, see [Change the workspace to which an exclusive resource group is bound]().

    ![Bind an exclusive resource group to a workspace](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9630852061/p139131.png)


## Step 2: Add data sources

1.  Go to the **Data Integration** page.

    1.  In the left-side navigation pane of the DataWorks console, click **Workspaces**.

    2.  Find the workspace you created and click **Data Integration** in the **Actions** column.

2.  In the left-side navigation pane of the Data Integration page, click **Connection**.

3.  In the left-side navigation pane of the page that appears, click **Data Source**. Then, click **New data source** in the upper-right corner.

4.  In the Relational Database section of the **Add data source** dialog box, click **DRDS**.

5.  In the **Add DRDS data source** dialog box, configure parameters and test connectivity. After the connectivity test is passed, click **Complete**.

    ![Add a DRDS data source](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0673343061/p134922.png)

    |Parameter|Description|
    |---------|-----------|
    |**Data source type**|This topic uses **Connection string mode** as an example. You can also select **Alibaba Cloud Database \(DRDS\)**. For more information, see [Configure a DRDS connection]().|
    |**Data Source Name**|The name of the data source. The name must contain letters, digits, and underscores \(\_\). It must start with a letter.|
    |**Description**|The description of the data source. The description cannot exceed 80 characters in length.|
    |**JDBC URL**|The JDBC URL of the database, in the format of jdbc:mysql://ServerIP:Port/Database. Replace ServerIP:Port with Endpoint of the VPC where the DRDS instance resides:Port number of the VPC. Replace Database with the name of the DRDS database you created.|
    |**User name**|The username that is used to connect to the database.|
    |**Password**|The password that is used to connect to the database.|

6.  Add an Elasticsearch data source in the same way.

    ![Add an Elasticsearch data source](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0673343061/p134923.png)

    |Parameter|Description|
    |---------|-----------|
    |**Data Source Name**|The name of the data source. The name must contain letters, digits, and underscores \(\_\). It must start with a letter.|
    |**Description**|The description of the data source. The description cannot exceed 80 characters in length.|
    |**Endpoint**|Set this parameter to a value in the format of http://<Internal endpoint of the Elasticsearch cluster\>:9200. You can obtain the internal endpoint from the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page of the cluster.|
    |**User name**|The username that is used to access the Elasticsearch cluster. The default username is **elastic**.|
    |**Password**|The password that corresponds to the username. The password of the **elastic** account is specified when you create the cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting a password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|


## Step 3: Create and run a data synchronization task

1.  On the Data Development tab of the DataWorks console, create a business process.

    For more information, see [Manage workflows]().

2.  In the navigation tree, right-click **Data Integration** and choose **New** \> **Offline synchronization**.

3.  In the **New node** dialog box, set **Node name** and click **Submit**.

4.  In the **Data source** section of the **Select data source** step, specify the DRDS data source and the name of the table you created. In the **Data destination** section, specify the Elasticsearch data source, index name, and index type.

    ![Specify data sources](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0673343061/p134948.png)

    **Note:**

    -   You can also use the script mode to configure data synchronization. For more information, see [Create a sync node by using the code editor](), [Configure DRDS Reader](), and [Configure Elasticsearch Writer]().
    -   We recommend that you set **Enable node discovery** to **No** in the **advanced configuration** of the **Elasticsearch** data source. Otherwise, a connection timeout error occurs during data synchronization.
5.  In the **Field Mapping** step, configure mappings between **source fields** and **target fields**.

    In this example, the default **source fields** are used. You only need to change **target fields**. Click the ![Change fields](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0673343061/p134953.png) icon next to **Target Field**. In the dialog box that appears, enter the following information:

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

    The following figure shows the configured field mappings.

    ![Configure field mappings](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0673343061/p134955.png)

6.  In the **Channel control** step, configure the parameters that are related to the execution of the synchronization task.

    In this example, use the default settings.

    ![Channel control](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0673343061/p134957.png)

    **Note:** If you want to schedule the task on a regular basis, configure the parameters based on the instructions in the [Basic properties]() topic.

7.  In the upper part of the page, click the ![Run icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0673343061/p134958.png) icon. Then, view the **operational log** of the task.

    **successfully** indicates that the task is successfully executed. **FINISH** indicates that the task is executed.

    ![Run the task](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0673343061/p134965.png)


## Step 4: View synchronization results

1.  Log on to the Kibana console of the destination Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to query the volume of data in the Elasticsearch cluster:

    **Note:** You can compare the queried data volume with the volume of data in the DRDS database to check whether all data is synchronized.

    ```
    GET drdstest/_search
    {
      "query": {
        "match_all": {}
      }
    }
    ```

    If the command is successfully executed, the result shown in the following figure is returned.

    ![Query data volume](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0673343061/p134970.png)

4.  Run the following command to search for data by using a specific field:

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

    If the command is successfully executed, the result shown in the following figure is returned.

    ![Field-based data search](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0673343061/p134972.png)


