---
keyword: SkyWalking
---

# Use SkyWalking to implement end-to-end monitoring on Alibaba Cloud Elasticsearch

SkyWalking is a distributed application performance monitoring \(APM\) tool and a distributed tracing system. This topic describes how to use SkyWalking to monitor an Alibaba Cloud Elasticsearch V7.4 cluster.

SkyWalking has the following features:

-   SkyWalking provides auto instrument agents so that you do not need to modify the application code.
-   SkyWalking provides manual instrument agents that support OpenTracing SDKs. Manual instrument agents can monitor the components supported by OpenTracing API for Java.

    **Note:** For more information about the components supported by OpenTracing API for Java, see [OpenTracing Registry](https://opentracing.io/registry/).

-   Auto and manual instrument agents can be used at the same time. Manual instrument agents can monitor the components that are not supported by auto instrument agents, and even private components.
-   SkyWalking is a Java-based backend program for analytics. It provides RESTful APIs and analytics capabilities for agents of other languages.
-   SkyWalking provides high-performance streaming analytics.

The following figure shows the SkyWalking architecture.

![SkyWalking architecture](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7657359951/p97734.png)

SkyWalking is a platform for storing data analytics and measurement results. These results are submitted to SkyWalking Collector over HTTP or gRPC. SkyWalking Collecter analyzes and aggregates data and stores the data in Elasticsearch, H2, MySQL, or TiDB. You can view the analysis results on the SkyWalking UI. SkyWalking collects data in different formats from multiple sources, such as SkyWalking agents in multiple programming languages, Zipkin v1, Zipkin v2, Istio telemetry, and Envoy.

**Note:** In this topic, SkyWalking is integrated into Alibaba Cloud Elasticsearch V7.4. You can also use a Skywalking client to report data to a Java application. For more information, see [Report Java application data through SkyWalking](/intl.en-US/Setup/Integrate Java Application/Report Java application data through SkyWalking.md). For more information about the middleware and components supported by SkyWalking, see [Apache SkyWalking documentation](https://github.com/opentracing-contrib/meta).

## Prerequisites

You have completed the following operations:

-   Create an Alibaba Cloud Elasticsearch cluster. In this topic, an Elasticsearch V7.4 cluster is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

-   Prepare a Linux server and install JDK 1.8 or later on the server.

    We recommend that you use an Alibaba Cloud Elastic Compute Service \(ECS\) instance. For more information, see [Step 1: Create an ECS instance](/intl.en-US/Quick Start/Create an instance in the ECS console (detailed version)/Quick start for Linux instances.md).

    **Note:** For information about how to install a JDK, see [Step 3: Install the JDK](/intl.en-US/Best Practices/Migrate and synchronize MySQL data/RDS synchronization/Use Canal to synchronize MySQL data to Alibaba Cloud Elasticsearch.md). If the JDK is not correctly installed and you start SkyWalking to view logs, the error message "Java not found" or "java-xxx: No such file or directory" is reported.

-   Make sure that ports 8080, 10800, 11800, and 12800 on the Linux server are not occupied.
-   Disable the firewall and Security-Enhanced Linux \(SELinux\) of the Linux server.

## Procedure

1.  [Step 1: Download and install SkyWalking](#section_kkj_d78_ojy)

2.  [Step 2: Configure SkyWalking to connect to Alibaba Cloud Elasticsearch](#section_2u5_xqx_gha)

3.  [Step 3: Verify the results](#section_dda_3ss_zv5)


## Step 1: Download and install SkyWalking

1.  Download the [SkyWalking](http://skywalking.apache.org/downloads/) package to the Linux server.

    We recommend that you select the latest version 7.0.0. An Elasticsearch V7.4 cluster is used in this topic. Therefore, you must select Binary Distribution for ElasticSearch 7. The following code provides the command that is used to download the package:

    ```
    wget https://mirror.bit.edu.cn/apache/skywalking/7.0.0/apache-skywalking-apm-es7-7.0.0.tar.gz
    ```

2.  Run the following command to decompress the package:

    ```
    tar -zxvf apache-skywalking-apm-es7-7.0.0.tar.gz
    ```

3.  Run the following command to view the decompressed files:

    ```
    ll apache-skywalking-apm-bin-es7/
    ```

    The following result is returned:

    ```
    total 92
    drwxrwxr-x 8 1001 1002   143 Mar 18 23:50 agent
    drwxr-xr-x 2 root root   241 Apr 10 16:03 bin
    drwxr-xr-x 2 root root   221 Apr 10 16:03 config
    -rwxrwxr-x 1 1001 1002 29791 Mar 18 23:37 LICENSE
    drwxrwxr-x 3 1001 1002  4096 Apr 10 16:03 licenses
    -rwxrwxr-x 1 1001 1002 32838 Mar 18 23:37 NOTICE
    drwxrwxr-x 2 1001 1002 12288 Mar 19 00:00 oap-libs
    -rw-rw-r-- 1 1001 1002  1978 Mar 18 23:37 README.txt
    drwxr-xr-x 3 root root    30 Apr 10 16:03 tools
    drwxr-xr-x 2 root root    53 Apr 10 16:03 webapp
    ```


## Step 2: Configure SkyWalking to connect to Alibaba Cloud Elasticsearch

1.  Run the following commands to open the application.yml file in the config folder:

    ```
    cd apache-skywalking-apm-bin-es7/config/
    vi application.yml
    ```

2.  Locate `storage`, change H2 to elasticsearch7, and configure the file based on the following instructions:

    ```
    storage:
      selector: ${SW_STORAGE:elasticsearch7}
      elasticsearch7:
        nameSpace: ${SW_NAMESPACE:"skywalking-index"}
        clusterNodes: ${SW_STORAGE_ES_CLUSTER_NODES:es-cn-4591kzdzk000i****.public.elasticsearch.aliyuncs.com:9200}
        protocol: ${SW_STORAGE_ES_HTTP_PROTOCOL:"http"}
       # trustStorePath: ${SW_SW_STORAGE_ES_SSL_JKS_PATH:"../es_keystore.jks"}
       # trustStorePass: ${SW_SW_STORAGE_ES_SSL_JKS_PASS:""}
        enablePackedDownsampling: ${SW_STORAGE_ENABLE_PACKED_DOWNSAMPLING:true} # Hour and Day metrics will be merged into minute index.
        dayStep: ${SW_STORAGE_DAY_STEP:1} # Represent the number of days in the one minute/hour/day index.
        user: ${SW_ES_USER:"elastic"}
        password: ${SW_ES_PASSWORD:"es_password"}
    ```

    **Note:** SkyWalking stores data in H2 by default. However, H2 does not support persistent data storage, and you must change H2 to Elasticsearch.

    |Parameter|Description|
    |---------|-----------|
    |`selector`|The storage selector. For this example, set the value to elasticsearch7.|
    |`nameSpace`|The namespace. The value of this parameter is used as the prefix for the names of all indexes in the Elasticsearch cluster.|
    |`clusterNodes`|The endpoint of the Elasticsearch cluster. Alibaba Cloud Elasticsearch is not in the same virtual private cloud \(VPC\) as SkyWalking. You must use the public endpoint to access the Elasticsearch cluster. For more information about how to obtain the public endpoint of the Elasticsearch cluster, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|
    |`user`|The username of the Elasticsearch cluster. The default username is elastic.|
    |`password`|The password of the Elasticsearch cluster. The password is specified when you create the Elasticsearch cluster.|

    **Note:** Specify only the username and password. Comment out trustStorePath and trustStorePass. Otherwise, the error message "NoSuchFileException:../es\_keystore.jks" is reported.

3.  Change the IP address or port number for listening.

    By default, SkyWalking communicates with Elasticsearch over port 12800 for RESTful API operations and over port 11800 for gRPC API operations. The IP address or port number can be changed in the core part of the application.yml file. In this topic, the default values are used.

    ```
    core:
      selector: ${SW_CORE:default}
      default:
        # Mixed: Receive agent data, Level 1 aggregate, Level 2 aggregate
        # Receiver: Receive agent data, Level 1 aggregate
        # Aggregator: Level 2 aggregate
        role: ${SW_CORE_ROLE:Mixed} # Mixed/Receiver/Aggregator
        restHost: ${SW_CORE_REST_HOST:0.0.0.0}
        restPort: ${SW_CORE_REST_PORT:12800}
        restContextPath: ${SW_CORE_REST_CONTEXT_PATH:/}
        gRPCHost: ${SW_CORE_GRPC_HOST:0.0.0.0}
        gRPCPort: ${SW_CORE_GRPC_PORT:11800}
    ```

4.  In the webapp folder, modify the configurations in the webapp.yml file.

    Default configurations are used in this topic. You can modify the configurations based on your business requirements.

    ```
    server:
      port: 8080
    collector:
      path: /graphql
      ribbon:
        ReadTimeout: 10000
        # Point to all backend's restHost:restPort, split by ,
        listOfServers: 127.0.0.1:12800
    ```


## Step 3: Verify the results

1.  Run the following commands to start SkyWalking on the Linux server:

    ```
    cd ../bin
    ./startup.sh
    ```

    **Note:**

    -   Make sure that your Elasticsearch cluster is started before you start SkyWalking.
    -   SkyWalking Collector and SkyWalking UI are also started by running the `startup.sh` command.
    If SkyWalking is started, the following result is returned:

    ```
    SkyWalking OAP started successfully!
    SkyWalking Web Application started successfully!
    ```

2.  Enter http://<IP address of the Linux server\>:8080/ in the address bar of your browser.

    ![Access SkyWalking](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7657359951/p97771.png)

    **Note:** If this is the first time you use SkyWalking to connect to Alibaba Cloud Elasticsearch, the startup is slow. This is because SkyWalking needs to create a large number of indexes in Elasticsearch. Before the creation is completed, the accessed page is blank. You can view the logs that are stored in `<SkyWalking installation path>logs/skywalking-oap-server.log` to check whether the creation is completed.

3.  Log on to the Kibana console of your Elasticsearch cluster. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md). Then, run the `GET _cat/indices?v` command to view index data.

    In the returned results, a large number of indexes whose names start with `skywalking-index` exist.

    ![Indexes whose names start with skywalking-index](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7657359951/p97778.png)


