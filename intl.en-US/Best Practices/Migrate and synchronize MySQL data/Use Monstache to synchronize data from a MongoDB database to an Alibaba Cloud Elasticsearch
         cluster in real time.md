---
keyword: [use Monstache to synchronize data in real time, synchronize data from a MongoDB database to an Elasticsearch cluster]
---

# Use Monstache to synchronize data from a MongoDB database to an Alibaba Cloud Elasticsearch cluster in real time

Alibaba Cloud Elasticsearch allows you to analyze semantics and displays analysis results in large charts for your business data that is stored in a MongoDB database. This topic describes how to use Monstache to synchronize data from the MongoDB database to an Elasticsearch cluster in real time. It also describes how to analyze the data and display analysis results.

The example in this topic demonstrates how to parse and collect statistics on data of popular movies. You can perform the following operations:

-   Use Monstache to quickly synchronize and subscribe to full or incremental data.
-   Synchronize data from your MongoDB database to an Elasticsearch cluster of a later version in real time.
-   Familiarize yourself with the common configuration parameters of Monstache.

## Benefits

-   The MongoDB database, Elasticsearch cluster, and Monstache are deployed in Virtual Private Clouds \(VPCs\). Data can be securely transmitted over internal networks at a high speed.
-   Monstache synchronizes and subscribes to data in real time based on MongoDB oplogs. It allows you to synchronize data between MongoDB databases and later versions of Elasticsearch clusters. Monstache supports the change streams and aggregation pipelines of MongoDB. For more information about Monstache features, see [Features](https://rwynn.github.io/monstache-site/).
-   Monstache supports logical and physical deletion. You can also use it to delete databases and collections. It can ensure data consistency between the Elasticsearch cluster and MongoDB database in real time.

## Procedure

1.  [Preparations](#section_jqd_7jz_x4p)

    Create an ApsaraDB for MongoDB instance, an Elasticsearch cluster, and an Elastic Compute Service \(ECS\) instance in the same VPC. The ECS instance is used to install Monstache.

    **Note:** Make sure that the version of Monstache you installed is compatible with the versions of the ApsaraDB for MongoDB instance and Elasticsearch cluster. For more information about version compatibility of Monstache, see [Monstache version](https://rwynn.github.io/monstache-site/start/).

2.  [Step 1: Build a Monstache environment](#section_5ti_1i2_n93)

    Install Monstache on the ECS instance. Before you install Monstache, make sure that you have configured Go environment variables.

3.  [Step 2: Configure a real-time data synchronization task](#section_q8c_w5h_4di)

    Modify information in the default configuration file of Monstache. The information includes the endpoints of the ApsaraDB for MongoDB instance and Elasticsearch cluster, the collections you want to synchronize, and the username and password of the Elasticsearch cluster. After you modify the preceding information, run Monstache to synchronize data from the ApsaraDB for MongoDB instance to the Elasticsearch cluster in real time.

4.  [Step 3: Verify the data synchronization result](#section_6ug_64d_cp6)

    Add, update, or remove data in an ApsaraDB for MongoDB instance. Then, check whether data is synchronized in real time.

5.  [Step 4: Analyze data and display analysis results in the Kibana console](#section_yuq_jwr_tc9)

    In the Kibana console, analyze data and display analysis results in pie charts.


## Preparations

1.  Create an Elasticsearch cluster and enable the Auto Indexing feature for the cluster.

    For more information, see [Create an Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Elasticsearch cluster.md) and [Enable auto indexing](/intl.en-US/Quick Start/Step 2 (optional): Configure a cluster.md). In this topic, an Elasticsearch V6.7.0 cluster of the Standard Edition is used.

2.  Create an ApsaraDB for MongoDB instance and prepare test data.

    For more information, see [Quick start](/intl.en-US/Quick Start for Replica Set/Create a replica set instance.md). In this topic, an ApsaraDB for MongoDB replica set instance of V4.2 is used. The following figure shows part of the test data.

    ![Test data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6900469951/p128772.png)

    **Note:** The ApsaraDB for MongoDB instance you use must be a replica set instance or sharded cluster instance.

3.  Create an ECS instance.

    For more information, see [Create an instance by using the provided wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the provided wizard.md). The ECS instance is used to install Monstache and must reside in the same VPC as the Elasticsearch cluster.


## Step 1: Build a Monstache environment

1.  Connect to the ECS instance.

    For more information, see [Connect to an ECS instance]().

2.  Install SDK for Go and configure environment variables.

    **Note:** Monstache-based data synchronization depends on the Go language. Therefore, before you install Monstache, you must prepare the Go environment on the ECS instance.

    1.  Download and decompress the installation package of SDK for Go.

        ```
        wget https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz
        tar -C /usr/local -xzf go1.14.4.linux-amd64.tar.gz
        ```

    2.  Configure environment variables.

        Run the `vim /etc/profile` command to open the configuration file for environment variables. Then, add the following content to the file. `GOPROXY` specifies a proxy for the modules of Alibaba Cloud SDK for Go.

        ```
        export GOROOT=/usr/local/go
        export GOPATH=/home/go/
        export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
        export GOPROXY=https://mirrors.aliyun.com/goproxy/
        ```

    3.  Apply the environment variables.

        ```
        source /etc/profile
        ```

3.  Install Monstache.

    1.  Go to the installation path.

        ```
        cd /usr/local/
        ```

    2.  Download the installation package from the GitHub repository.

        ```
        git clone https://github.com/rwynn/monstache.git
        ```

    3.  Go to the monstache directory.

        ```
        cd monstache
        ```

    4.  Switch the version.

        In this topic, rel5 is used.

        ```
        git checkout rel5
        ```

    5.  Install Monstache.

        ```
        go install
        ```

    6.  View the version of Monstache.

        ```
        monstache -v
        ```

        If the command is successfully executed, the following result is returned:

        ```
        5.5.5
        ```


## Step 2: Configure a real-time data synchronization task

[Monstache](https://rwynn.github.io/monstache-site/start/#usage) uses the TOML format for its configuration. In most cases, Monstache uses the default port to connect to the Elasticsearch cluster and ApsaraDB for MongoDB instance on your local host and tracks the oplogs of the ApsaraDB for MongoDB instance. During the running of Monstache, all changes to data in your ApsaraDB for MongoDB database are synchronized to the Elasticsearch cluster.

In this topic, ApsaraDB for MongoDB and Alibaba Cloud Elasticsearch are used, and you need to specify the objects that you want to synchronize. Therefore, you must modify the default configuration file of Monstache. The objects that are used in this topic are the hotmovies and col collections in the mydb database. To modify the configuration file, perform the following steps:

1.  Go to the Monstache installation directory, create a configuration file, and edit the file.

    ```
    cd /usr/local/monstache/
    vim config.toml
    ```

2.  Modify the configuration file.

    The following example demonstrates how to modify the configuration file. For more information, see [Monstache Usage](https://rwynn.github.io/monstache-site/start/#usage).

    ```
    # connection settings
    
    # connect to MongoDB using the following URL
    mongo-url = "mongodb://root:<your_mongodb_password>@dds-bp1aadcc629******.mongodb.rds.aliyuncs.com:3717"
    # connect to the Elasticsearch REST API at the following node URLs
    elasticsearch-urls = ["http://es-cn-mp91kzb8m00******.elasticsearch.aliyuncs.com:9200"]
    
    # frequently required settings
    
    # if you need to seed an index from a collection and not just listen and sync changes events
    # you can copy entire collections or views from MongoDB to Elasticsearch
    direct-read-namespaces = ["mydb.hotmovies","mydb.col"]
    
    # if you want to use MongoDB change streams instead of legacy oplog tailing use change-stream-namespaces
    # change streams require at least MongoDB API 3.6+
    # if you have MongoDB 4+ you can listen for changes to an entire database or entire deployment
    # in this case you usually don't need regexes in your config to filter collections unless you target the deployment.
    # to listen to an entire db use only the database name.  For a deployment use an empty string.
    #change-stream-namespaces = ["mydb.col"]
    
    # additional settings
    
    # if you don't want to listen for changes to all collections in MongoDB but only a few
    # e.g. only listen for inserts, updates, deletes, and drops from mydb.mycollection
    # this setting does not initiate a copy, it is only a filter on the change event listener
    #namespace-regex = '^mydb\.col$'
    # compress requests to Elasticsearch
    #gzip = true
    # generate indexing statistics
    #stats = true
    # index statistics into Elasticsearch
    #index-stats = true
    # use the following PEM file for connections to MongoDB
    #mongo-pem-file = "/path/to/mongoCert.pem"
    # disable PEM validation
    #mongo-validate-pem-file = false
    # use the following user name for Elasticsearch basic auth
    elasticsearch-user = "elastic"
    # use the following password for Elasticsearch basic auth
    elasticsearch-password = "<your_es_password>"
    # use 4 go routines concurrently pushing documents to Elasticsearch
    elasticsearch-max-conns = 4
    # use the following PEM file to connections to Elasticsearch
    #elasticsearch-pem-file = "/path/to/elasticCert.pem"
    # validate connections to Elasticsearch
    #elastic-validate-pem-file = true
    # propogate dropped collections in MongoDB as index deletes in Elasticsearch
    dropped-collections = true
    # propogate dropped databases in MongoDB as index deletes in Elasticsearch
    dropped-databases = true
    # do not start processing at the beginning of the MongoDB oplog
    # if you set the replay to true you may see version conflict messages
    # in the log if you had synced previously. This just means that you are replaying old docs which are already
    # in Elasticsearch with a newer version. Elasticsearch is preventing the old docs from overwriting new ones.
    #replay = false
    # resume processing from a timestamp saved in a previous run
    resume = true
    # do not validate that progress timestamps have been saved
    #resume-write-unsafe = false
    # override the name under which resume state is saved
    #resume-name = "default"
    # use a custom resume strategy (tokens) instead of the default strategy (timestamps)
    # tokens work with MongoDB API 3.6+ while timestamps work only with MongoDB API 4.0+
    resume-strategy = 0
    # exclude documents whose namespace matches the following pattern
    #namespace-exclude-regex = '^mydb\.ignorecollection$'
    # turn on indexing of GridFS file content
    #index-files = true
    # turn on search result highlighting of GridFS content
    #file-highlighting = true
    # index GridFS files inserted into the following collections
    #file-namespaces = ["users.fs.files"]
    # print detailed information including request traces
    verbose = true
    # enable clustering mode
    cluster-name = 'es-cn-mp91kzb8m00******'
    # do not exit after full-sync, rather continue tailing the oplog
    #exit-after-direct-reads = false
    [[mapping]]
    namespace = "mydb.hotmovies"
    index = "hotmovies"
    type = "movies"
    
    [[mapping]]
    namespace = "mydb.col"
    index = "mydbcol"
    type = "collection"
    ```

    |Parameter|Description|
    |---------|-----------|
    |mongo-url|The connection string of the primary node in the ApsaraDB for MongoDB instance. You can obtain the connection string from the details page of the ApsaraDB for MongoDB instance. Before you obtain the connection string, add the internal IP address of the ECS instance where Monstache is installed to the whitelist of the ApsaraDB for MongoDB instance. For more information, see [Configure a whitelist for a sharded cluster instance](/intl.en-US/Quick Start for Cluster/Configure a whitelist for a sharded cluster instance.md).|
    |elasticsearch-urls|The endpoint of the Elasticsearch cluster. Specify the endpoint in the format of `http://<Internal endpoint of the Elasticsearch cluster>:9200`. You can obtain the internal endpoint from the details page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|
    |direct-read-namespaces|The collections that you want to synchronize. For more information, see [direct-read-namespaces](https://rwynn.github.io/monstache-site/config/#direct-read-namespaces). The collections used in this topic are hotmovies and col in the mydb database.|
    |change-stream-namespaces|You must specify this parameter if you want to use the change streams of the ApsaraDB for MongoDB instance. If you specify this parameter, oplog tracking becomes invalid. For more information, see [change-stream-namespaces](https://rwynn.github.io/monstache-site/config/#change-stream-namespaces).|
    |namespace-regex|The regular expression that is used to specify the collections you want to monitor. After you specify a regular expression, the system can monitor changes to data in collections that match the regular expression.|
    |elasticsearch-user|The username that is used to access the Elasticsearch cluster. The default value is elastic. **Note:** For security purposes, we recommend that you do not use the elastic account. You can use a custom account. Before you use a custom account, you must create a role for it and grant required permissions to the role. For more information, see [Create a role](/intl.en-US/RAM/Manage Kibana role/Create a role.md) and [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md). |
    |elasticsearch-password|The password of the elastic account. The password is specified when the Elasticsearch cluster is created. If you forget the password, you can reset it. For more information about how to reset a password and related precautions, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|
    |elasticsearch-max-conns|The number of threads that are used to connect to the Elasticsearch cluster. The default value is 4. This value indicates that four Go threads are used at a time to synchronize data to the Elasticsearch cluster.|
    |dropped-collections|The default value is true. This value indicates that Monstache deletes a mapped index in the Elasticsearch cluster if a collection in your ApsaraDB for MongoDB database is deleted.|
    |dropped-databases|The default value is true. This value indicates that Monstache deletes mapped indexes in the Elasticsearch cluster if your ApsaraDB for MongoDB database is deleted.|
    |resume|The default value is false. If you set this parameter to true, Monstache writes the timestamps of operations that are synchronized to the Elasticsearch cluster to the monstache.monstache collection. If Monstache fails, you can use the timestamps to resume the synchronization task. This avoids data loss. If Monstache starts with the cluster-name parameter specified, the resume parameter is automatically set to true. For more information, see [resume](https://rwynn.github.io/monstache-site/config/#resume).|
    |resume-strategy|The resuming policy. This parameter is valid only when resume is set to true. For more information, see [resume-strategy](https://rwynn.github.io/monstache-site/config/#resume-strategy).|
    |verbose|The default value is false. This value indicates that log debugging is disabled.|
    |cluster-name|The name of the cluster. If you specify this parameter, Monstache runs in high availability mode. Processes that have the same cluster name cooperate with each other. For more information, see [cluster-name](https://rwynn.github.io/monstache-site/config/#cluster-name).|
    |mapping|The mapping of the index in the Elasticsearch cluster. By default, when data is synchronized from a MongoDB database to an Elasticsearch cluster, the index name is automatically mapped to `Database name.Collection name`. You can change this index name by setting this parameter. For more information, see [Index Mapping](https://rwynn.github.io/monstache-site/advanced/#index-mapping).|

    **Note:** Monstache supports a large number of parameters. The preceding table describes only some parameters that are used for real-time data synchronization. For more information about how to configure parameters that are used for complex data synchronization, see [Monstache config](https://rwynn.github.io/monstache-site/config/#configuration) and [Advanced](https://rwynn.github.io/monstache-site/advanced/#index-mapping).

3.  Run Monstache.

    ```
    monstache -f config.toml
    ```

    **Note:** The -f parameter is used to explicitly run Monstache. In this case, the system will log all debugging operations, including request tracking for the Elasticsearch cluster.


## Step 3: Verify the data synchronization result

1.  Log on to the Data Management \(DMS\) console of the ApsaraDB for MongoDB instance and the Kibana console of the Elasticsearch cluster. Query the number of documents before the synchronization and that after the synchronization.

    **Note:**

    -   For more information about how to log on to the DMS console, see [Connect to a replica set ApsaraDB for MongoDB instance through DMS](/intl.en-US/Quick Start for Replica Set/Connect to an instance/Connect to a replica set ApsaraDB for MongoDB instance through DMS.md).
    -   For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).
    -   ApsaraDB for MongoDB instance

        ```
        db.hotmovies.find().count()
        ```

        If the command is successfully executed, the following result is returned:

        ```
        [
        10000
        ]
        ```

    -   Elasticsearch cluster

        ```
        GET hotmovies/_count
        ```

        If the command is successfully executed, the following result is returned:

        ```
        {
          "count" : 10000,
          "_shards" : {
            "total" : 5,
            "successful" : 5,
            "skipped" : 0,
            "failed" : 0
          }
        }
        ```

2.  Insert data into the ApsaraDB for MongoDB database and check whether the data is synchronized to the Elasticsearch cluster.

    -   ApsaraDB for MongoDB instance

        ```
        db.hotmovies.insert({id: 11003,title: "Beauty",overview: "How a group of IT women with high IQ become outstanding",original_language:"cn",release_date:"2020-06-17",popularity:67.654,vote_count:65487,vote_average:9.9})
        ```

        ```
        db.hotmovies.insert({id: 11004,title: "Heroic Programmers",overview: "How a group of IT men with high IQ become outstanding",original_language:"cn",release_date:"2020-06-15",popularity:77.654,vote_count:85487,vote_average:11.9})
        ```

    -   Elasticsearch cluster

        ```
        GET hotmovies/_search
        {
          "query": {
            "bool": {
              "should": [
                {"term":{"id":"11003"}},
                {"term":{"id":"11004"}}
              ]
            }
          }
        }
        ```

        ![Insert data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6900469951/p128730.png)

3.  Update data in the ApsaraDB for MongoDB database. Then, check whether the data in the Elasticsearch cluster is also updated.

    -   ApsaraDB for MongoDB instance

        ```
        db.hotmovies.update({'title':'Beauty'},{$set:{'title':'Beautiful Programmers'}})
        ```

    -   Elasticsearch cluster

        ```
        GET hotmovies/_search
        {
          "query": {
            "match": {
              "id":"11003"
            }
          }
        }
        ```

        ![Update data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6900469951/p128722.png)

4.  Remove data from the ApsaraDB for MongoDB database. Then, check whether the data is also removed from the Elasticsearch cluster.

    -   ApsaraDB for MongoDB instance

        ```
        db.hotmovies.remove({id: 11003})
        ```

        ```
        db.hotmovies.remove({id: 11004})
        ```

    -   Elasticsearch cluster

        ```
        GET hotmovies/_search
        {
          "query": {
            "bool": {
              "should": [
                {"term":{"id":"11003"}},
                {"term":{"id":"11004"}}
              ]
            }
          }
        }
        ```

        ![Remove data](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6900469951/p128733.png)


## Step 4: Analyze data and display analysis results in the Kibana console

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  Create an index pattern.

    ![Create an index pattern](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6900469951/p128741.png)

    1.  In the left-side navigation pane, click **Management**.

    2.  In the **Kibana** section, click **Index Patterns**.

    3.  Click **Create index pattern**.

    4.  Specify **Index pattern** and click **Next step**.

    5.  Specify **Time Filter field name**. In this example, Time Filter field name is set to **I don't want to use the Time Filter**.

    6.  Click **Create index pattern**.

3.  Configure a chart.

    The following example demonstrates how to configure a pie chart for top 10 popular movies:

    1.  In the left-side navigation pane, click **Visualize**.

    2.  Click **+** next to the search box.

    3.  In the **New Visualization** dialog box, click **Pie**.

        ![Create a pie chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6900469951/p128759.png)

    4.  Click the **hotmovies** index pattern.

        ![Click the index pattern](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6900469951/p128766.png)

    5.  Configure parameters in the **Metrics** and **Buckets** sections based on the following figure.

        ![Pie chart configuration](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6900469951/p128764.png)

    6.  Click the ![Run icon](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6900469951/p128763.png) icon to apply the configurations and display data.

        ![Pie chart](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6900469951/p128765.png)


