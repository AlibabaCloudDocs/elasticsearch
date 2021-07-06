---
keyword: use Realtime Compute for Apache Flink and Elasticsearch to build a log retrieval system
---

# Use Realtime Compute to process and synchronize data to an Alibaba Cloud Elasticsearch cluster

If you want to build a log retrieval system, you can use Alibaba Cloud Realtime Compute for Apache Flink to compute log data and import the processed data into Alibaba Cloud Elasticsearch for searches. This topic uses log data in Log Service to describe the detailed procedure.

You have completed the following operations:

-   Activate Realtime Compute and create a project.

    For more information, see [Activate Realtime Compute for Apache Flink and create a project](/intl.en-US/Exclusive Mode/Preparation/Activate Realtime Compute for Apache Flink and create a project.md).

-   Create an Elasticsearch cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

-   Activate Log Service, and create a project and a Logstore.

    For more information, see [Quick start](/intl.en-US/.md), [Create a project](/intl.en-US/Preparation/Manage a project.md), and [Create a Logstore](/intl.en-US/Preparation/Manage a Logstore.md).


Realtime Compute for Apache Flink is a Flink-based service provided by Alibaba Cloud. It supports various input and output systems, such as Kafka and Elasticsearch. You can use Realtime Compute for Apache Flink and Elasticsearch to retrieve logs.

Realtime Compute for Apache Flink processes logs in Kafka or Log Service by using simple or complex Flink SQL statements. Then, it imports the processed logs into an Elasticsearch cluster as source data for searches. The computing capabilities of Realtime Compute for Apache Flink and the search capabilities of Elasticsearch allow you to process and search for data in real time. This help you transform your business into real-time services.

Realtime Compute for Apache Flink provides a simple way to interact with Elasticsearch. For example, logs or data records are imported into Log Service and must be processed before they are imported into an Elasticsearch cluster. The following figure shows the data consumption pipeline.

![Data consumption pipeline between Realtime Compute for Apache Flink and Elasticsearch](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2557359951/p62597.png)

## Procedure

1.  Log on to the [Realtime Compute development platform](https://stream-ap-southeast-3.console.aliyun.com).

2.  Create a Realtime Compute job.

    For more information, see [Create a job](/intl.en-US/Exclusive Mode/Flink SQL Development Guide/Data development/Develop a job.md).

3.  Write Flink SQL statements.

    1.  Create a source table for Log Service.

        ```
        create table sls_stream(
          a int,
          b int,
          c VARCHAR
        )
        WITH (
          type ='sls',  
          endPoint ='<yourEndpoint>',
          accessId ='<yourAccessId>',
          accessKey ='<yourAccessKey>',
          startTime = '<yourStartTime>',
          project ='<yourProjectName>',
          logStore ='<yourLogStoreName>',
          consumerGroup ='<yourConsumerGroupName>'
        );
        ```

        The following table describes the parameters in the WITH part.

        |Parameter|Description|
        |---------|-----------|
        |endPoint|The URL that is used to access projects and logs in Log Service. For more information, see [Endpoints](/intl.en-US/Developer Guide/API Reference/Endpoints.md). For example, the URL that is used to access Log Service in the China \(Hangzhou\) region is **http://cn-hangzhou.log.aliyuncs.com**. Make sure that the URL starts with **http://**. |
        |accessId|The AccessKey ID of your Alibaba Cloud account.|
        |accessKey|The AccessKey secret of your Alibaba Cloud account.|
        |startTime|The time when logs start to be consumed. When you run a Realtime Compute for Apache Flink job, specify a time point that is later than the start time specified by this parameter.|
        |project|The name of the Log Service project.|
        |logStore|The name of the Logstore in the project.|
        |consumerGroup|The name of the Log Service consumer group.|

        For more information about other parameters in the WITH part, see [Create a Log Service source table](/intl.en-US/Exclusive Mode/Flink SQL/DDL statements/Create a source table/Create a Log Service source table.md).

    2.  Create an Elasticsearch result table.

        **Note:**

        -   Elasticsearch result tables are supported in Realtime Compute V3.2.2 and later. When you create a Realtime Compute for Apache Flink job, select a valid version.
        -   Elasticsearch result tables are based on RESTful APIs and are compatible with all Elasticsearch versions.
        ```
        CREATE TABLE es_stream_sink(
          a int,
          cnt BIGINT,
          PRIMARY KEY(a)
        )
        WITH(
          type ='elasticsearch',
          endPoint = 'http://<instanceid>.public.elasticsearch.aliyuncs.com:<port>',
          accessId = '<yourAccessId>',
          accessKey = '<yourAccessSecret>',
          index = '<yourIndex>',
          typeName = '<yourTypeName>'
        );
        ```

        The following table describes the parameters in the WITH part.

        |Parameter|Description|
        |---------|-----------|
        |endPoint|The URL that is used to access the Elasticsearch cluster over the Internet. Specify the URL in the format of http://<instanceid\>.public.elasticsearch.aliyuncs.com:9200. You can obtain the public endpoint of the cluster from the Basic Information page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|
        |accessId|The username that is used to access the Elasticsearch cluster. The default username is elastic.|
        |accessKey|The password that is used to access the Elasticsearch cluster. The password of the elastic account is specified when you create the cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting a password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|
        |index|The name of the destination index. If no indexes are created in the Elasticsearch cluster, create one first. For more information, see [Step 3: Create an index](/intl.en-US/Elasticsearch Instances Management/Quick start.md). You can also enable the Auto Indexing feature for the Elasticsearch cluster to automatically create indexes. For more information, see [Configure the YML file](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md).|
        |typeName|The type of the destination index. The index type of Elasticsearch clusters of V7.0 or later must be \_doc.|

        For more information about other parameters in the WITH part, see [Create an Elasticsearch result table](/intl.en-US/Exclusive Mode/Flink SQL/DDL statements/Create a result table/Create an Elasticsearch result table.md).

        **Note:**

        -   Elasticsearch allows you to update documents based on the `PRIMARY KEY` field. Only one field can be specified as the `PRIMARY KEY` field. If you specify the `PRIMARY KEY` field, values of the `PRIMARY KEY` field are used as document IDs. If the `PRIMARY KEY` field is not specified, the system generates random IDs for documents. For more information, see [Index API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html).
        -   Elasticsearch supports multiple update modes. You can set the `updateMode` parameter to specify the update mode.
            -   If `updateMode` is set to full, new documents overwrite existing documents.
            -   If `updateMode` is set to inc, new values overwrite existing values of the related fields.
        -   All updates in Elasticsearch are performed by using INSERT or UPDATE statements that follow the UPSERT syntax.
    3.  Create data consumption logic and synchronize data.

        ```
        INSERT INTO es_stream_sink
        SELECT 
          a,
          count(*) as cnt
        FROM sls_stream GROUP BY a
        ```

4.  Submit and run the job.

    For more information, see [Publish a job](/intl.en-US/Exclusive Mode/Flink SQL Development Guide/Data development/Publish a job.md) and [Start a job](/intl.en-US/Exclusive Mode/Flink SQL Development Guide/Data development/Start a job.md).

    After you submit and run the job, data stored in Log Service is aggregated and imported into the Elasticsearch cluster. Realtime Compute for Apache Flink also supports other compute operations. For more information, see [Overview](/intl.en-US/Exclusive Mode/Flink SQL/Overview.md).


## Summary

Realtime Compute for Apache Flink and Elasticsearch allow you to quickly create your own real-time search services. If more complex logic is required to import data into an Elasticsearch cluster, use the user-defined sinks of Realtime Compute for Apache Flink. For more information, see [Create a custom result table](/intl.en-US/Exclusive Mode/Flink SQL/DDL statements/Create a result table/Create a custom result table.md).

