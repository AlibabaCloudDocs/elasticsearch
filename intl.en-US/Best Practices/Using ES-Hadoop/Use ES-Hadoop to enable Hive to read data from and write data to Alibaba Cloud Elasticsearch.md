# Use ES-Hadoop to enable Hive to read data from and write data to Alibaba Cloud Elasticsearch

ES-Hadoop is a tool developed by open source Elasticsearch. It connects Elasticsearch to Apache Hadoop and enables data transmission between them. ES-Hadoop combines the quick search capability of Elasticsearch and the batch processing capability of Hadoop to achieve interactive data processing. This topic describes how to use ES-Hadoop to enable Hive to read data from and write data to Alibaba Cloud Elasticsearch.

Apache Hadoop can handle large datasets. However, when it is used for interactive analytics, a high latency occurs. Elasticsearch has an advantage over Apache Hadoop in interactive analytics. It can respond to queries, especially ad hoc queries, within seconds. ES-Hadoop has combined the advantages of both Hadoop and Elasticsearch. ES-Hadoop allows you to make only a few code modifications to process the data that is stored in Alibaba Cloud Elasticsearch. ES-Hadoop also provides an accelerated query experience.

ES-Hadoop uses Elasticsearch as a data source of data processing engines such as MapReduce, Spark, and Hive. ES-Hadoop also uses Elasticsearch as storage in a compute-storage separation architecture. Elasticsearch works in a similar way to other data sources of MapReduce, Spark, and Hive. However, Elasticsearch can select and filter data in a more rapid manner. This is critical to an analytics engine.

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

    An Elasticsearch V6.7.0 cluster is used in this topic. For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

2.  Disable the Auto Indexing feature for the cluster. Create an index in the cluster and configure mappings for the index.

    If you enable the Auto Indexing feature for the cluster, the index that is automatically created by the Elasticsearch cluster may not meet your requirements. For example, you define a field age of the INT type and enable the Auto Indexing feature. In this case, the data type of the age field may become LONG in the index. Therefore, we recommend that you disable the Auto Indexing feature. An index named company is created in this topic. The following code shows this index and its mappings:

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

    **Note:** By default, 0.0.0.0/0 is specified in the internal IP address whitelist of the Elasticsearch cluster. You can view the whitelist configuration on the cluster security configuration page. If the default setting is not used, you must add the internal IP address of the EMR cluster to the whitelist.

    -   For more information, see [View the cluster list and cluster details](/intl.en-US/Cluster Management/Configure clusters/View the cluster list and cluster details.md).
    -   For more information about how to configure an internal IP address whitelist for access to the Elasticsearch cluster over a VPC, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).
4.  Download an [ES-Hadoop package](https://www.elastic.co/cn/downloads/hadoop) that is compatible with the version of the Elasticsearch cluster.

    The elasticsearch-hadoop-6.7.0.zip package is used in this topic.


## Step 1: Upload the ES-Hadoop JAR package to HDFS

1.  Log on to the [EMR console](https://emr.console.aliyun.com/) and obtain the IP address of the master node in your EMR cluster. Then, use SSH to log on to the Elastic Compute Service \(ECS\) instance that is indicated by the IP address.

    For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Upload the elasticsearch-hadoop-6.7.0.zip package to the master node. Decompress the package to obtain the elasticsearch-hadoop-6.7.0.jar file.

3.  Create an HDFS directory and upload the elasticsearch-hadoop-6.7.0.jar file to the directory.

    ```
    hadoop fs -mkdir /tmp/hadoop-es
    hadoop fs -put /tmp/hadoop-es/elasticsearch-hadoop-6.7.0.jar /tmp/hadoop-es
    ```


## Step 2: Create a Hive external table

1.  On the **Data Platform** tab of the EMR console, create a **HiveSQL** job.

    For more information, see [Configure a Hive SQL job](/intl.en-US/Data Development/Jobs/Configure a Hive SQL job.md).

    ![Create a HiveSQL job](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9414216061/p170551.png)

2.  Configure the job and create a Hive external table.

    The following code shows the configuration of the job:

    ```
    ####Add a JAR file, which only takes effect for the current session.########
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
        'es.nodes' = ' http://es-cn-mp91kzb8m0009****.elasticsearch.aliyuncs.com',
        'es.port' = '9200',
        'es.net.ssl' = 'true', 
        'es.nodes.wan.only' = 'true', 
        'es.nodes.discovery'='false',
        'es.input.json' = 'false',
        'es.resource' = 'company/employees',
        'es.net.http.auth.user' = 'elastic', 
        'es.net.http.auth.pass' = 'xxxxxx'
    );
    ```

    |Parameter|Default value|Description|
    |---------|-------------|-----------|
    |es.nodes|localhost|Specifies the endpoint that is used to access the Elasticsearch cluster. We recommend that you use the internal endpoint. You can obtain the internal endpoint on the Basic Information page of the Elasticsearch cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|
    |es.port|9200|The port number that is used to access the Elasticsearch cluster.|
    |es.net.http.auth.user|/|The username that is used to access the Elasticsearch cluster.|
    |es.net.http.auth.pass|/|The password that is used to access the Elasticsearch cluster.|
    |es.nodes.wan.only|false|Specifies whether to enable node sniffing when the Elasticsearch cluster uses a virtual IP address for connections. Valid values:    -   true: indicates that node sniffing is enabled.
    -   false: indicates that node sniff is disabled. |
    |es.nodes.discovery|true|Specifies whether to prohibit the node discovery mechanism. Valid values:    -   true: indicates that the node discovery mechanism is prohibited.
    -   false: indicates that the node discovery mechanism is not prohibited. |
    |es.index.auto.create|yes|Specifies whether to enable the Auto Indexing feature for the Elasticsearch cluster when Hadoop writes data to the cluster. Valid values:    -   true: indicates that the feature is enabled.
    -   false: indicates that the feature is disabled. |
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

3.  If the job is successfully run, log on to the Kibana console of the Elasticsearch cluster to query the data in the company index.

    For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md). You can run the following command to query the data in the company index:

    ```
    GET company/_search
    ```

    If the command is executed successfully, the result shown in the following figure is returned.

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

