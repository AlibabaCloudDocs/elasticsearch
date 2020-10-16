# Use Alibaba Cloud Elasticsearch and Rsbeat to analyze Redis slow logs in real time

Redis is a widely used key-value database that delivers high performance. Redis is single threaded. Inappropriate use of Redis may cause slow queries. Excessive slow queries or a slow query that takes a long time, such as 20 seconds, may block an operation queue or cause services to be unavailable. In this case, you must collect and analyze Redis slow logs in real time to locate and handle exceptions. This topic describes how to use Alibaba Cloud Elasticsearch and Rsbeat to analyze Redis slow logs in real time.

You can use Rsbeat to collect and send Redis slow logs to Elasticsearch. Then, use graphs to analyze the logs in the Kibana console. Terms:

-   Elasticsearch: a Lucene-based, distributed, and real-time search and analytics engine. It is an open source product released under the Apache License. Elasticsearch is a popular search engine for enterprises. Elasticsearch provides distributed services and allows you to store, query, and analyze large amounts of datasets in near real time. It is typically used as a basic engine or technology to support complex queries and high-performance applications.

    Alibaba Cloud Elasticsearch is compatible with open source Elasticsearch features, such as Security, Machine Learning, Graph, and Application Performance Management \(APM\). Alibaba Cloud Elasticsearch is released in 5.5.3, 6.3.2, 6.7.0, 6.8.0, and 7.4.0 versions. It supports the commercial plug-in X-Pack and is ideal for scenarios such as data analytics and searches. Based on the features of open source Elasticsearch, Alibaba Cloud Elasticsearch implements enterprise-grade access control, security monitoring and alerting, and automated reporting. Alibaba Cloud Elasticsearch is used in this topic. Click for a [free trial](https://common-buy.aliyun.com/?spm=a2c4g.11186623.2.20.7f9818738QhPyy&commodityCode=elasticsearchpre&request=%7B%22region%22:%22cn-hangzhou%22,%22es_version%22:%225.5.3_with_X-Pack%22,%22network_type%22:%22VPC%22,%22vs_area%22:%22cn-hangzhou-b%22,%22vpc_id%22:%22vpc-bp170psqmu5is7iml6bq9%22,%22vswitch_id%22:%22vsw-bp1jyxgwodxsb1h9tfbih%22,%22node_spec%22:%22elasticsearch.n4.small%22,%22disk%22:20,%22node_amount%22:2,%22dedicate_master%22:false,%22ord_time%22:%22%5B%5Cn%20%201,%5Cn%20%20%5C%22Month%5C%22,%5Cn%20%20null%5Cn%5D%22%7D).

-   Rsbeat: a data shipper that is used to collect and analyze Redis slow logs. For more information, see [Rsbeat documentation](https://github.com/yourdream/rsbeat).
-   Redis: an open source, in-memory data structure store. It can be used as a database, cache, and messaging middleware. For more information, see [open source Redis documentation](https://redis.io/).

    ApsaraDB for Redis is a database service that is compatible with native Redis protocols. It supports hybrid storage that combines memory and hard disks. ApsaraDB for Redis provides a high-availability hot standby architecture and can scale to meet requirements for read and write operations that require high performance. ApsaraDB for Redis is used in this topic. For more information, see [What is ApsaraDB for Redis?](/intl.en-US/Product Introduction/What is ApsaraDB for Redis?.md).


## Procedure

1.  [Preparations](#section_m28_k8s_ntf)

    Create an Alibaba Cloud Elasticsearch cluster, an ApsaraDB for Redis instance, and an Elastic Compute Service \(ECS\) instance in the same virtual private cloud \(VPC\).

2.  [Step 1: Configure slow query parameters for the ApsaraDB for Redis instance.](#section_hiv_s7s_9gq)

    Configure the conditions to generate Redis slow logs and specify the maximum number of slow logs that can be recorded based on your requirements.

3.  [Step 2: Install and configure Rsbeat](#section_jj2_pgp_dde)

    Install Rsbeat on the ECS instance and specify the ApsaraDB for Redis instance and Alibaba Cloud Elasticsearch cluster you created in the Rsbeat configuration file.

4.  [Step 3: Analyze the Redis slow logs in the Kibana console by using graphs](#section_gdb_u5k_a4g)

    View the details about the Redis slow logs in the Kibana console and analyze the logs as required.


## Preparations

1.  Create an Alibaba Cloud Elasticsearch cluster and enable the Auto-indexing feature for the cluster.

    For more information, see [Create an Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Elasticsearch cluster.md) and [Enable auto indexing](/intl.en-US/Quick Start/Step 2 (optional): Configure a cluster.md). An Elasticsearch V6.7 cluster of the Standard Edition is used in this topic.

2.  Create an ApsaraDB for Redis instance.

    For more information, see [Step 1: Create an ApsaraDB for Redis instance](/intl.en-US/Quick Start/Step 1: Create an ApsaraDB for Redis instance.md). An ApsaraDB for Redis V5.0 instance of the Community Edition is used in this topic. This instance resides in the same VPC as the Elasticsearch cluster. This allows you to access the Elasticsearch cluster over an internal network.

3.  Create an ECS instance.

    For more information, see [Create an instance by using the provided wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the provided wizard.md). An ECS instance that runs an image of 64-bit CentOS 7.6 is used in this topic. This instance resides in the same VPC as the ApsaraDB for Redis instance and Elasticsearch cluster.

4.  Configure a whitelist for access to the ApsaraDB for Redis instance.

    Add the internal IP address of the ECS instance to the whitelist of the ApsaraDB for Redis instance. For more information, see [Set IP address whitelists](/intl.en-US/User Guide/Security management/Set IP address whitelists.md).


## Step 1: Configure slow query parameters for the ApsaraDB for Redis instance.

1.  Log on to the [ApsaraDB for Redis console](https://kvstore.console.aliyun.com/).

2.  In the top navigation bar, select a region.

3.  On the **Instances** page, find your desired ApsaraDB for Redis instance and click its ID or click **Manage** in the **Actions** column.

4.  In the left-side navigation pane, click **System Parameters**.

5.  In the System Parameters section, find the **slowlog-log-slower-than** and **slowlog-max-len** parameters. Then, modify these parameters based on your requirements.

    |Parameter|Description|Example|
    |---------|-----------|-------|
    |**slowlog-log-slower-than**|When the runtime of a command exceeds the value of this parameter, the command is defined as a slow query and recorded as a slow log. The runtime does not include the time spent in queuing. Unit: microseconds. Default value: 10000 \(10 milliseconds\). **Note:** If you set this parameter to a negative number, ApsaraDB for Redis does not record slow queries as slow logs. If you set this parameter to 0, ApsaraDB for Redis records all commands.

|This parameter is set to 20000 in this topic. This value indicates that a command whose runtime exceeds 20 milliseconds is recorded as a slow log.|
    |**slowlog-max-len**|The maximum number of slow query commands that can be recorded as slow logs. If the number of commands that are recorded exceeds the value of this parameter, ApsaraDB for Redis deletes the earliest slow logs.|This parameter is set to 100 in this topic. This value indicates that ApsaraDB for Redis records the latest 100 slow query commands as slow logs.|


## Step 2: Install and configure Rsbeat

1.  Connect to the ECS instance.

    For more information, see [Connect to an ECS instance](/intl.en-US/Instance/Connect to instances/Connect to Linux instances/Connect to a Linux instance by using VNC.md).

2.  Install Rsbeat.

    Rsbeat 5.3.2 is used in this topic.

    ```
    wget https://github.com/Yourdream/rsbeat/archive/master.zip
    unzip master.zip
    ```

3.  Modify the configuration of Rsbeat.

    1.  Run the following command to open the rsbeat.yml file.

        ```
        cd rsbeat-master
        vim rsbeat.yml
        ```

    2.  Modify the configurations in the rsbeat and output.elasticsearch sections based on the following instructions and save the modifications.

        ![Configurations in the rsbeat.yml file](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9215742061/p131929.png)

        |Parameter|Description|
        |---------|-----------|
        |period|The interval at which Rsbeat sends Redis slow logs to the Elasticsearch cluster.|
        |redis|The endpoint that is used to connect to the ApsaraDB Redis instance. For more information, see [View endpoints](/intl.en-US/User Guide/Connection management/View endpoints.md). **Note:** The password that is used to access the ApsaraDB for Redis instance is not specified in the configuration file of Rsbeat. To enable Rsbeat to access the ApsaraDB for Redis instance, you must enable password-free access after you obtain the endpoint. For more information, see [Enable password-free access](/intl.en-US/User Guide/Security management/Enable password-free access.md). |
        |slowerThan|The time that is required to send the `config set slowlog-log-slower-than` command to the Redis server. Unit: microseconds.|

        |Parameter|Description|
        |---------|-----------|
        |hosts|The endpoint that is used to access the Elasticsearch cluster. You can obtain the endpoint on the Basic Information page of the Elasticsearch cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|
        |username|The username that is used to access the Elasticsearch cluster. Default value: elastic.|
        |password|The password that corresponds to the username. The password is specified when you create your Elasticsearch cluster. If you forget the password, you can reset it. For more information about precautions and procedures for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|
        |template.overwrite|Specifies whether the Rsbeat-created index template that has the same name as the index template of the Elasticsearch cluster overwrites the index template of the Elasticsearch cluster. Default value: true.|

4.  Start the Rsbeat service.

    ```
    ./rsbeat.linux.amd64 -c rsbeat.yml -e -d "*"
    ```


## Step 3: Analyze the Redis slow logs in the Kibana console by using graphs

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  Create an index pattern.

    1.  In the left-side navigation pane, click **Management**.

    2.  In the **Kibana** section, click **Index Patterns**.

    3.  Click **Create index pattern**.

    4.  In the section that appears, enter an index pattern name in the **Index pattern** field and click **Next step**.

        ![Enter an index pattern name](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9215742061/p132004.png)

    5.  Specify **Time Filter field name**. In this topic, this parameter is set to **@timestamp**.

    6.  Click **Create index pattern**.

3.  View the details about the Redis slow logs.

    1.  In the left-side navigation pane, click **Discover**.

    2.  In the left part of the page, select rsbeat-\* from the drop-down list below **Add a filter**.

    3.  In the upper-right corner of the page, select a time range and view the details about the Redis slow logs during the time range.

        ![View the details about the Redis slow logs](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9215742061/p132012.png)

4.  Record the top 10 keys with the largest number of Redis slow logs and arrange them in descending order.

    1.  In the left-side navigation pane, click **Visualize**.

    2.  On the **Visualize** page, click the ![Add icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9215742061/p132045.png) icon.

    3.  In the **New Visualization** dialog box, click **Pie**.

        ![Click Pie](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9215742061/p132052.png)

    4.  In the From a New Search, Select Index section, click rsbeat-\*.

        ![Select an index pattern](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9215742061/p132051.png)

    5.  Configure Metrics and Buckets based on the following figure.

        ![Configurations of Metrics and Buckets](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9215742061/p132049.png)

    6.  Click the ![Run icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9215742061/p132050.png) icon to view the result.

        ![Result](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0315742061/p132048.png)

        **Note:** For more information about the usage notes of the Kibana console, see [Kibana User Guide](https://www.elastic.co/guide/en/kibana/6.7/index.html).


