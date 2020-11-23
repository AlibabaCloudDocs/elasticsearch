---
keyword: use Canal to synchronize MySQL data to Elasticsearch
---

# Use Canal to synchronize MySQL data to Alibaba Cloud Elasticsearch

Canal is an open source product provided by Alibaba Group. It can parse incremental log data in MySQL and allows you to subscribe to and consume incremental data. You can use Canal to synchronize incremental data from a MySQL database to an Alibaba Cloud Elasticsearch cluster. This topic describes the procedure in detail. An ApsaraDB RDS for MySQL database is used in this topic.

## Procedure

1.  [Preparations](#section_e69_vic_nli)

    Create an ApsaraDB RDS for MySQL instance, an Alibaba Cloud Elasticsearch cluster, and an Elastic Compute Service \(ECS\) instance.

    **Note:** The ApsaraDB RDS for MySQL instance, Alibaba Cloud Elasticsearch cluster, and ECS instance used in this topic reside in the same virtual private cloud \(VPC\).

    -   ApsaraDB RDS for MySQL instance: stores source data and incremental data.
    -   Canal: an open source extract, transform, load \(ETL\) software provided in GitHub. It is used to parse database logs, obtain incremental data, and synchronize the incremental data to the Alibaba Cloud Elasticsearch cluster. For more information, see [Canal](https://github.com/alibaba/canal).
    -   Alibaba Cloud Elasticsearch cluster: receives incremental data.
    -   ECS instance: used to deploy the Canal server and Canal adapter.
2.  [Step 1: Prepare the MySQL data source](#section_bya_9h5_d4u)

    Prepare the data that you want to synchronize in the ApsaraDB RDS for MySQL instance.

3.  [Step 2: Create an index and configure mappings](#section_tt7_ab3_ll2)

    Create an index and configure mappings for the index in the Elasticsearch cluster. The field names and types that are defined in the mappings must be the same as those of the data that you want to synchronize.

4.  [Step 3: Install the JDK](#section_1uk_499_551)

    Before you use Canal, you must install the Java Development Kit \(JDK\). The version of the JDK must be 1.8.0 or later.

5.  [Step 4: Install and start the Canal server](#section_8jx_g9s_pto)

    Install the Canal server and modify its configuration file to associate it with the ApsaraDB RDS for MySQL instance. For a MySQL cluster, the Canal server simulates a slave node in the cluster to obtain the binary logs on the master node in the cluster. Then, the Canal server sends the logs to the Canal adapter.

6.  [Step 5: Install and start the Canal adapter](#section_2tp_qfz_qw5)

    Install the Canal adapter and modify its configuration file to associate it with the ApsaraDB RDS for MySQL instance and Elasticsearch cluster. Then, define the mappings between the fields in the ApsaraDB RDS for MySQL instance and those in the Elasticsearch cluster for data synchronization.

7.  [Step 6: Verify the synchronization result of incremental data](#section_fxm_zna_2n1)

    Add, modify, or delete data in the ApsaraDB RDS for MySQL instance and view the data synchronization result.


## Preparations

-   Create an ApsaraDB RDS for MySQL instance.

    For more information, see [Create an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create an ApsaraDB RDS for MySQL instance.md). The following figure shows the configuration of the ApsaraDB RDS for MySQL instance used in this topic.

    ![ApsaraDB RDS for MySQL configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6457359951/p59221.png)

-   Create an Alibaba Cloud Elasticsearch cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md). An **Elasticsearch V6.7 cluster** of the **Standard Edition** is created in this topic.

    **Note:** Canal cannot be used to synchronize data to Elasticsearch clusters of V7.0 or later.

-   Create an Alibaba Cloud ECS instance.

    For more information, see [Step 1: Create an ECS instance](/intl.en-US/Quick Start/Create an instance in the ECS console (detailed version)/Quick start for Linux instances.md). The ECS instance runs an **image** of **CentOS 7.6 64-bit** in this topic.


## Step 1: Prepare the MySQL data source

Log on to the [ApsaraDB RDS console](https://rdsnext.console.aliyun.com). Create an ApsaraDB RDS for MySQL database and a table. For more information, see [Quick start of ApsaraDB RDS for MySQL](/intl.en-US/RDS MySQL Database/Quick start/General workflow to use RDS for MySQL.md).

In this topic, table **es\_test** is created. The following figure shows the fields that are added to the table.

![Fields and indexes of es_test](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6457359951/p59264.png)

## Step 2: Create an index and configure mappings

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to create an index and configure mappings.

    **Note:** The field names and types specified in the command must be the same as those in the table created in [Step 1: Prepare the MySQL data source](#section_bya_9h5_d4u).

    ```
    PUT es_test? include_type_name=true
    {
    
        "settings" : {
          "index" : {
            "number_of_shards" : "5",
            "number_of_replicas" : "1"
          }
        },
        "mappings" : {
            "_doc" : {
                "properties" : {
                  "count": {          
                       "type": "text"       
                   },
                  "id": {
                       "type": "integer"
                   },
                    "name" : {
                        "type" : "text",
                       "analyzer": "ik_smart"                   
                    },
                    "color" : {
                        "type" : "text"                    
                    }
                }
            }
        }
    }
    ```

    If the index and mappings are configured, the following result is returned:

    ```
    {
      "acknowledged" : true,
      "shards_acknowledged" : true,
      "index" : "es_test"
    }
    ```


## Step 3: Install the JDK

1.  Connect to the ECS instance and query available JDK packages.

    ```
    yum search java | grep -i --color JDK
    ```

2.  Install the JDK of the required version.

    **java-1.8.0-openjdk-devel.x86\_64** is used in this topic.

    ```
    yum install java-1.8.0-openjdk-devel.x86_64
    ```

3.  Configure environment variables.

    1.  Open the profile file in the etc folder.

        ```
        vi /etc/profile
        ```

    2.  Add the following environment variables to the file:

        ```
        export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.71-2.b15.el7_2.x86_64
        export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
        export PATH=$PATH:$JAVA_HOME/bin
        ```

    3.  Press **Esc**, and run the `:wq` command to save the file and exit from the vi mode. Then, run the following command to apply the configuration:

        ```
        source /etc/profile
        ```

4.  Run the following commands to check whether the JDK is installed:

    -   `java`
    -   `javac`
    -   `java -version`
    If the JDK is installed, the result shown in the following figure is returned.

    ![JDK installed](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6457359951/p59240.png)


## Step 4: Install and start the Canal server

1.  Download the Canal server package.

    A Canal V1.1.4 server is used in this topic.

    ```
    wget https://github.com/alibaba/canal/releases/download/canal-1.1.4/canal.deployer-1.1.4.tar.gz
    ```

2.  Run the following command to decompress the package:

    ```
    tar -zxvf canal.deployer-1.1.4.tar.gz
    ```

3.  Run the following command to modify the conf/example/instance.properties file:

    ```
    vi conf/example/instance.properties
    ```

    ![Modify the conf/example/instance.properties file](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6457359951/p59251.png)

    |Parameter|Description|
    |---------|-----------|
    |`canal.instance.master.address`|Specify this parameter in the format of `<Internal endpoint of the ApsaraDB RDS for MySQL database>:<Internal port>`. You can obtain the required information on the **Basic Information** page of the ApsaraDB RDS for MySQL instance. Example: `rm-bp1u1xxxxxxxxx6ph.mysql.rds.aliyuncs.com:3306`.|
    |`canal.instance.dbUsername`|The username that is used to log on to the ApsaraDB RDS for MySQL database. You can obtain the username on the **Accounts** tab of the ApsaraDB RDS for MySQL instance.|
    |`canal.instance.dbPassword`|The password that is used to log on to the ApsaraDB RDS for MySQL database.|

4.  Press **Esc**, and run the `:wq` command to save the file and exit from the vi mode.

5.  Start the Canal server and query the log.

    ```
    ./bin/startup.sh
    cat logs/canal/canal.log
    ```

    ![Start the Canal server](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7457359951/p59254.png)


## Step 5: Install and start the Canal adapter

1.  Download the Canal adapter package.

    A Canal V1.1.4 adapter is used in this topic.

    ```
    wget https://github.com/alibaba/canal/releases/download/canal-1.1.4/canal.adapter-1.1.4.tar.gz
    ```

2.  Run the following command to decompress the package:

    ```
    tar -zxvf canal.adapter-1.1.4.tar.gz
    ```

3.  Run the following command to modify the conf/application.yml file:

    ```
    vi conf/application.yml
    ```

    ![Modify the conf/application.yml file](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7457359951/p59278.png)

    |Parameter|Description|
    |---------|-----------|
    |`canal.conf.canalServerHost`|The endpoint of the Canal deployer. Retain the default value `127.0.0.1:11111`.|
    |`canal.conf.srcDataSources.defaultDS.url`|Specify this parameter in the format of `jdbc:mysql://<Internal endpoint of the ApsaraDB RDS for MySQL database>:<Internal port>/<Database name>? useUnicode=true`. You can obtain the required information on the **Basic Information** page of the ApsaraDB RDS for MySQL instance. Example: `jdbc:mysql://rm-bp1xxxxxxxxxnd6ph.mysql.rds.aliyuncs.com:3306/elasticsearch? useUnicode=true`.|
    |`canal.conf.srcDataSources.defaultDS.username`|The username that is used to log on to the ApsaraDB RDS for MySQL database. You can obtain the username on the **Accounts** tab of the ApsaraDB RDS for MySQL instance.|
    |`canal.conf.srcDataSources.defaultDS.password`|The password that is used to log on to the ApsaraDB RDS for MySQL database.|
    |`canal.conf.canalAdapters.groups.outerAdapters.hosts`|Find `name:es` and replace `hosts` with `<Internal endpoint of the Elasticsearch cluster>:<Internal port>`. You can obtain the required information on the Basic Information page of the Elasticsearch cluster. Example: `es-cn-v64xxxxxxxxx3medp.elasticsearch.aliyuncs.com:9200`.|
    |`canal.conf.canalAdapters.groups.outerAdapters.mode`|Set the value to `rest`.|
    |`canal.conf.canalAdapters.groups.outerAdapters.properties.security.auth`|Specify this parameter in the format of `<Username of the Elasticsearch cluster>:<Password>`. Example: `elastic:es_password`.|
    |`canal.conf.canalAdapters.groups.outerAdapters.properties.cluster.name`|The ID of the Elasticsearch cluster. You can obtain the cluster ID on the Basic Information page of the Elasticsearch cluster. Example: `es-cn-v64xxxxxxxxx3medp`.|

4.  Press **Esc**, and run the `:wq` command to save the file and exit from the vi mode.

5.  Repeat the preceding steps to modify the conf/es/\*.yml file and specify the fields that you want to map from the ApsaraDB RDS for MySQL database to the Elasticsearch cluster.

    ![Modify the conf/es/*.yml file](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7457359951/p59280.png)

    |Parameter|Description|
    |---------|-----------|
    |`esMapping._index`|Set the value to the name of the index created in the Elasticsearch cluster in [Step 2: Create an index and configure mappings](#section_tt7_ab3_ll2). **es\_test** is used in this topic.|
    |`esMapping._type`|Set the value to the type of the index created in the Elasticsearch cluster in [Step 2: Create an index and configure mappings](#section_tt7_ab3_ll2). **\_doc** is used in this topic.|
    |`esMapping._id`|The ID of the document generated for the fields that you want to synchronize to the Elasticsearch cluster. You can customize this parameter. **\_id** is used in this topic.|
    |`esMapping.sql`|The SQL statement that is used to query the fields that you want to synchronize to the Elasticsearch cluster. The `select t.id as _id,t.id,t.count,t.name,t.color from es_test t` statement is used in this topic.|

6.  Start the Canal adapter and query logs.

    ```
    ./bin/startup.sh
    cat logs/adapter/adapter.log
    ```

    If the Canal adapter is started, the result shown in the following figure is returned.

    ![Canal adapter service logs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7457359951/p59329.png)


## Step 6: Verify the synchronization result of incremental data

1.  In the ApsaraDB RDS for MySQL database, add, modify, or remove data in the **es\_test** table.

    ```
    insert `elasticsearch`.`es_test`(`count`,`id`,`name`,`color`) values('11',2,'canal_test2','red');
    ```

2.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

3.  In the left-side navigation pane, click **Dev Tools**. On the **Console** tab of the page that appears, run the following command to query the synchronized data:

    ```
    GET /es_test/_search
    ```

    If the incremental data is synchronized, the result shown in the following figure is returned.

    ![Successful data synchronization](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7457359951/p59328.png)


