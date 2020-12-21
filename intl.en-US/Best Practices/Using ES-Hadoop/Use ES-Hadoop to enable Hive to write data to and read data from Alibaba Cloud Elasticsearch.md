# Use ES-Hadoop to enable Hive to write data to and read data from Alibaba Cloud Elasticsearch

Elasticsearch-Hadoop \(ES-Hadoop\) is a tool developed by open source Elasticsearch. It connects Elasticsearch to Apache Hadoop and enables data transmission between them. ES-Hadoop combines the quick search capability of Elasticsearch and the batch processing capability of Hadoop to achieve interactive data processing. This topic describes how to use ES-Hadoop to enable Hive to write data to and read data from Alibaba Cloud Elasticsearch.

Hadoop can handle large datasets. However, when it is used for interactive analytics, a high latency occurs. Elasticsearch has an advantage over Hadoop in interactive analytics. It can respond to queries, especially ad hoc queries, within seconds. ES-Hadoop combines the advantages of Hadoop and Elasticsearch. ES-Hadoop allows you to make only a few code modifications to process the data that is stored in Elasticsearch. ES-Hadoop also provides an accelerated query experience.

ES-Hadoop uses Elasticsearch as a data source of data processing engines, such as MapReduce, Spark, and Hive. ES-Hadoop also uses Elasticsearch as storage in a computing-storage separation architecture. Elasticsearch works in a similar way to other data sources of MapReduce, Spark, and Hive. However, Elasticsearch can select and filter data in a more rapid manner. This is critical to an analytics engine.

![ES-Hadoop architecture](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9414216061/p170380.png)

## Procedure

1.  [Preparations](#section_frh_f2a_68a)

    Create an Alibaba Cloud Elasticsearch cluster and an E-MapReduce \(EMR\) cluster in the same virtual private cloud \(VPC\). Disable the Auto Indexing feature for the Elasticsearch cluster. Create an index in the Elasticsearch cluster and configure mappings for the index. Download the ES-Hadoop package that is compatible with the version of the Elasticsearch cluster.

2.  [Step 1: Upload the ES-Hadoop JAR package to HDFS](#section_ztf_2er_3tm)

    Upload the ES-Hadoop package to the HDFS directory on the master node of the EMR cluster.

3.  [Step 2: Create a Hive external table](#section_0ia_99w_0ze)

    Create a Hive external table and map the fields in the table with those in the index of the Elasticsearch cluster.

4.  [Step 3: Use Hive to write data to the index](#section_o64_yl2_28o)

    Use HiveSQL to write data to the index of the Elasticsearch cluster.

5.  [Step 4: Use Hive to read data from the index](#section_ahk_ejx_7ep)

    Use HiveSQL to read data from the index of the Elasticsearch cluster.


## Preparations

1.  Create an Alibaba Cloud Elasticsearch cluster.

    In this topic, an Elasticsearch V6.7.0 cluster is created. For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

2.  Disable the Auto Indexing feature for the cluster. Create an index in the cluster and configure mappings for the index.

    If you enable the Auto Indexing feature for the cluster, the index that is automatically created by the Elasticsearch cluster may not meet your requirements. For example, you define the age field of the INT data type and enable the Auto Indexing feature. In this case, the data type of the age field may become LONG in the index. Therefore, we recommend that you disable the Auto Indexing feature. An index named company is created in this topic. The following code shows this index and its mappings:

    ```
    PUT company
    {
      "mappings": {
        "_doc": {
          "properties": {
            "id": {
              "type": "long"
            },
            "name": {
              "type": "text",
              "fields": {
                "keyword": {
                  "type": "keyword",
                  "ignore_above": 256
                }
              }
            },
            "birth": {
              "type": "text"
            },
            "addr": {
              "type": "text"
            }
          }
        }
      },
      "settings": {
        "index": {
          "number_of_shards": "5",
          "number_of_replicas": "1"
        }
      }
    }
    ```

3.  Create an EMR cluster that resides in the same VPC as the Elasticsearch cluster.

    **Note:** By default, 0.0.0.0/0 is specified in the private IP address whitelist of the Elasticsearch cluster. You can view the whitelist configuration on the cluster security configuration page. If the default setting is not used, you must add the private IP address of the EMR cluster to the whitelist.

    -   For more information about how to obtain the private IP address of the EMR cluster, see [View the cluster list and cluster details](/intl.en-US/Cluster Management/Configure clusters/View the cluster list and cluster details.md).
    -   For more information about how to configure the private IP address whitelist of the Elasticsearch cluster, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md). The IP addresses in the whitelist can be used to access the Elasticsearch cluster over a VPC.
4.  Download an [ES-Hadoop package](https://www.elastic.co/cn/downloads/hadoop) that is compatible with the version of the Elasticsearch cluster.

    The elasticsearch-hadoop-6.7.0.zip package is used in this topic.


## Step 1: Upload the ES-Hadoop JAR package to HDFS

1.  Log on to the [EMR console](https://emr.console.aliyun.com/) and obtain the IP address of the master node of the EMR cluster. Then, use SSH to log on to the Elastic Compute Service \(ECS\) instance that is indicated by the IP address.

    For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Upload the elasticsearch-hadoop-6.7.0.zip package to the master node. Decompress the package to obtain the elasticsearch-hadoop-hive-6.7.0.jar file.

3.  Create an HDFS directory and upload the elasticsearch-hadoop-hive-6.7.0.jar file to the directory.

    ```
    hadoop fs -mkdir /tmp/hadoop-es
    hadoop fs -put elasticsearch-hadoop-6.7.0/dist/elasticsearch-hadoop-hive-6.7.0.jar /tmp/hadoop-es
    ```


## Step 2: Create a Hive external table

1.  On the **Data Platform** tab of the EMR console, create a **HiveSQL** job.

    For more information, see [Configure a Hive SQL job](/intl.en-US/Data Development/Jobs/Configure a Hive SQL job.md).

    ![Create a HiveSQL job](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9414216061/p170551.png)

2.  Configure the job and create a Hive external table.

    The following code shows the configuration of the job:

    ```
    ####Add a JAR file, which is valid only for the current session.########
    add jar hdfs:///tmp/hadoop-es/elasticsearch-hadoop-hive-6.7.0.jar;
    ####Create a Hive external table and map the table with the index of the Elasticsearch cluster.####
    CREATE EXTERNAL table IF NOT EXISTS company( 
       id BIGINT,
       name STRING,
       birth STRING,
       addr STRING 
    )  
    STORED BY 'org.elasticsearch.hadoop.hive.EsStorageHandler' 
    TBLPROPERTIES(  
        'es.nodes' = 'http://es-cn-mp91kzb8m0009****.elasticsearch.aliyuncs.com',
        'es.port' = '9200',
        'es.net.ssl' = 'true', 
        'es.nodes.wan.only' = 'true', 
        'es.nodes.discovery'='false',
        'es.input.use.sliced.partitions'='false',
        'es.input.json' = 'false',
        'es.resource' = 'company/_doc',
        'es.net.http.auth.user' = 'elastic', 
        'es.net.http.auth.pass' = 'xxxxxx'
    );
    ```

    |Parameter|Default value|Description|
    |---------|-------------|-----------|
    |es.nodes|localhost|The endpoint that is used to access the Elasticsearch cluster. We recommend that you use the internal endpoint. You can obtain the internal endpoint on the Basic Information page of the Elasticsearch cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|
    |es.port|9200|The port number that is used to access the Elasticsearch cluster.|
    |es.net.http.auth.user|elastic|The username that is used to access the Elasticsearch cluster.**Note:** If you use the elastic account to access your Elasticsearch cluster and then reset the password of the account, it may require some time for the new password to take effect. During this period, you cannot use the elastic account to access the cluster. Therefore, we recommend that you do not use the elastic account to access an Elasticsearch cluster. You can log on to the Kibana console and create a user with the required role to access an Elasticsearch cluster. For more information, see [Create a role](/intl.en-US/RAM/Manage Kibana role/Create a role.md) and [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md). |
    |es.net.http.auth.pass|/|The password that is used to access the Elasticsearch cluster.|
    |es.nodes.wan.only|false|Specifies whether to enable node sniffing when the Elasticsearch cluster uses a virtual IP address for connections. Valid values:    -   true: indicates that node sniffing is enabled.
    -   false: indicates that node sniff is disabled. |
    |es.nodes.discovery|true|Specifies whether to prohibit the node discovery mechanism. Valid values:    -   true: indicates that the node discovery mechanism is prohibited.
    -   false: indicates that the node discovery mechanism is not prohibited.
**Note:** If you use Alibaba Cloud Elasticsearch, you must set this parameter to false. |
    |es.input.use.sliced.partitions|true|Specifies whether to use partitions. Valid values:    -   true: indicates that partitions are used. In this case, more time may be required for the index read-ahead phase. The time required for this phase may be longer than the time required for data queries. To improve query efficiency, we recommend that you set this parameter to false.
    -   false: indicates that partitions are not used. |
    |es.index.auto.create|true|Specifies whether the system creates an index in the Elasticsearch cluster when you use ES-Hadoop to write data to the cluster. Valid values:    -   true: indicates that the system creates an index in the Elasticsearch cluster.
    -   false: indicates that the system does not create an index in the Elasticsearch cluster. |
    |es.resource|/|The name and type of the index on which data read or write operations are performed.|
    |es.mapping.names|/|The mappings between the field names in the table and those in the index of the Elasticsearch cluster.|
    |es.read.metadata|false|Specifies whether to include the document metadata such as **\_id** in the results. To include the document metadata, set the value to true.|

    For more information about the configuration items of ES-Hadoop, see [open source ES-Hadoop configuration](https://www.elastic.co/guide/en/elasticsearch/hadoop/current/configuration.html).

3.  Save and run the job.

    ![Save and run the job](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9414216061/p170585.png)

    If the job is successfully run, the result shown in the following figure is returned.

    ![Returned result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9414216061/p170593.png)


## Step 3: Use Hive to write data to the index

1.  Create a **HiveSQL** data write job.

    The following code shows the configuration of the job:

    ```
    add jar hdfs:///tmp/hadoop-es/elasticsearch-hadoop-hive-6.7.0.jar;
    INSERT INTO TABLE company VALUES (1, "zhangsan", "1990-01-01","No.969, wenyixi Rd, yuhang, hangzhou");
    INSERT INTO TABLE company VALUES (2, "lisi", "1991-01-01", "No.556, xixi Rd, xihu, hangzhou");
    INSERT INTO TABLE company VALUES (3, "wangwu", "1992-01-01", "No.699 wangshang Rd, binjiang, hangzhou");
    ```

2.  Save and run the job.

    ![Save and run the data write job](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9414216061/p170600.png)

3.  If the job is successfully run, log on to the Kibana console of the Elasticsearch cluster and query the data in the company index.

    For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md). You can run the following command to query the data in the company index:

    ```
    GET company/_search
    ```

    If the command is successfully run, the result shown in the following figure is returned.

    ![Data write result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9414216061/p170608.png)


## Step 4: Use Hive to read data from the index

1.  Create a **HiveSQL** data read job.

    The following code shows the configuration of the job:

    ```
    add jar hdfs:///tmp/hadoop-es/elasticsearch-hadoop-hive-6.7.0.jar;
    select * from company;
    ```

2.  Save and run the job.

    ![Save and run the data read job](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9414216061/p170614.png)


## Summary

This topic describes how to enable Hive to read and write data by using ES-Hadoop. Alibaba Cloud EMR and Elasticsearch are used in this topic. Data read and write by using Hive achieve more flexible data analytics. For more information about the advanced configurations of ES-Hadoop and Hive, see [open source Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/hadoop/6.8/hive.html).

