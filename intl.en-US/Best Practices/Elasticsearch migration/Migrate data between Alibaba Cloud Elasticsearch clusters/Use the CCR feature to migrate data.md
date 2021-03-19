# Use the CCR feature to migrate data

This topic describes how to use the cross-cluster replication \(CCR\) feature to migrate data between a local Alibaba Cloud Elasticsearch cluster and a remote Alibaba Cloud Elasticsearch cluster.

## Precautions

Due to the adjustment made to the Alibaba Cloud Elasticsearch network architecture, clusters created after October 2020 do not support the X-Pack Watcher and LDAP authentication features. You cannot reindex, search for, and replicate data between a cluster created before October 2020 and a cluster created after October 2020. You can perform the operations only between clusters created before October 2020 or between clusters created after October 2020. The features will be available soon.

## Background information

CCR is a [commercial feature](https://www.elastic.co/cn/subscriptions) released in open source Elasticsearch Platinum. After you [purchase an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md), you can use this feature free of charge based on a few simple configurations. Only single-zone Elasticsearch clusters of V6.7.0 or later support this feature. CCR is used in the following scenarios:

-   Disaster recovery and high availability

    You can use CCR to back up data among Elasticsearch clusters that reside in different regions. If a cluster fails, you can retrieve its index data from other clusters. This prevents data loss.

-   Data access from a nearby cluster

    For example, Company A has multiple subsidiaries that are located in different regions. To speed up business processing, you can plan the business of the subsidiaries based on their geographical locations. Then, use CCR to distribute business data to Elasticsearch clusters in different regions. Each subsidiary can directly use the cluster in the region where the subsidiary is located to process business.

-   Centralized reporting

    You can use CCR to replicate data from multiple small clusters to one cluster. Then, you can perform visualized analytics and reporting for the data in a centralized manner.


To use CCR, you must prepare two types of clusters: local clusters and remote clusters. Remote clusters provide source data, which is stored in leader indexes. Local clusters replicate the data and store it in follower indexes. You can also use CCR to migrate large volumes of data at a time in real time. For more information, see [Cross-cluster replication](https://www.elastic.co/guide/en/elasticsearch/reference/current/xpack-ccr.html).

## Procedure

1.  [Preparations](#section_tg8_ot6_nyu)

    Prepare a local cluster, a remote cluster, and a leader index.

2.  [Step 1: Connect clusters](#section_dsn_urt_lnd)

    Connect the remote cluster to the local cluster.

3.  [Step 2: Add the remote cluster](#section_op9_634_jh1)

    In the Kibana console of the local cluster, add the remote cluster.

4.  [Step 3: Configure CCR](#section_jz5_ir8_epc)

    In the Kibana console of the local cluster, configure the leader index and a follower index.

5.  [Step 4: View migration results](#section_qlz_hg6_yc7)

    Insert data into the remote cluster. Then, verify the data migration on the local cluster.


## Preparations

1.  Create a local cluster and a remote cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md). The two clusters must be single-zone clusters and belong to both the same virtual private cloud \(VPC\) and vSwitch. The versions of the two clusters must be the same \(V6.7.0 or later\).

2.  [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) of the remote cluster and create a leader index.

    **Note:**

    -   If you create an index in an Elasticsearch cluster of V7.0 or earlier, you must enable the soft\_deletes attribute. Otherwise, an error is reported.
    -   If you want to migrate data in an existing index, you can call the reindex operation to enable the soft\_deletes attribute.
    ```
    PUT myindex
    {
      "settings": {
        "index.soft_deletes.retention.operations": 1024,
        "index.soft_deletes.enabled": true
      }
    }
    ```

3.  Disable the physical replication feature for the leader index.

    The [physical replication](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the physical replication feature of the apack plug-in.md) feature is automatically enabled for indexes in Elasticsearch V6.7.0 clusters. Before you use CCR, you must disable the physical replication feature.

    1.  Disable the index.

        ```
        POST myindex/_close
        ```

    2.  Update the settings configuration of the index to disable the physical replication feature.

        ```
        PUT myindex/_settings
        {
        "index.replication.type" : null
        }
        ```

    3.  Enable the index.

        ```
        POST myindex/_open
        ```


## Step 1: Connect clusters

Configure the remote cluster to connect it to the local cluster. For more information, see [Connect Elasticsearch clusters](/intl.en-US/Elasticsearch Instances Management/Security/Connect Elasticsearch clusters.md). If the two clusters are connected, the information shown in the following figure appears.

![Connect clusters](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9201852061/p133815.png)

## Step 2: Add the remote cluster

1.  Log on to the Kibana console of the local cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Management**.

3.  In the **Elasticsearch** section, click **Remote Clusters**.

4.  Click **Add a remote cluster**.

5.  In the **Add remote cluster** section, configure the following parameters.

    ![Add remote cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9201852061/p133844.png)

    -   **Name**: the name of the remote cluster. The name must be unique.
    -   **Seed nodes**: the nodes in the remote cluster. Specify each node in the format of Node IP address:9300. To obtain the IP addresses of nodes, log on to the Kibana console of the remote cluster and run the `GET /_cat/nodes? v` command on the Console tab of the Dev Tools page. The nodes you specify must include a dedicated master node of the remote cluster. We recommend that you specify multiple nodes. This way, if the dedicated master node fails, you can still use CCR.

        ![Obtain the IP address of a node](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9201852061/p133849.png)

        **Note:** During CCR, Kibana uses the IP addresses of data nodes to access clusters over TCP port 9300. HTTP port 9200 is not supported.

6.  Click **Save**.

    The system then automatically connects to the remote cluster. If the connection is established, **Connected** appears.

    ![Connected](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9201852061/p133856.png)


## Step 3: Configure CCR

1.  Log on to the Kibana console of the local cluster. In the left-side navigation pane, click **Management**. In the **Elasticsearch** section of the page that appears, click **Cross Cluster Replication**.

2.  On the Follower indices tab, click **Create a follower index**.

3.  In the **Add follower index** section, configure the following parameters.

    ![Configure CCR](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9201852061/p133858.png)

    |Parameter|Description|
    |---------|-----------|
    |**Remote cluster**|Select the cluster you added in [Step 2: Add the remote cluster](#section_op9_634_jh1).|
    |**Leader index**|The index whose data you want to migrate. In this example, the **myindex** index that is created in [Preparations](#section_tg8_ot6_nyu) is used.|
    |**Follower index**|The index to which data is migrated. You must specify a unique index name.|

4.  Click **Create**.

    After the follower index is created, the index is in the **Active** state.

    ![Index status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9201852061/p134052.png)


## Step 4: View migration results

1.  [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) of the remote cluster and insert data into the remote cluster.

    ```
    POST myindex/_doc/
    {
      "name":"Jack",
      "age":40
    }
    ```

2.  Run the following command in the Kibana console of the local cluster to check whether the inserted data is migrated to the local cluster:

    ```
    GET myindex_follow/_search
    ```

    If the running of the command is successful, the result shown in the following figure is returned.

    ![Data migration results](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9201852061/p134056.png)

    The preceding figure shows that data in the leader index myindex of the remote cluster is migrated to the follower index myindex\_follow of the local cluster.

    **Note:** The follower index myindex\_follow is read-only. If you want to write data to the follower index, convert the follower index into a common index first. For more information, see [Use Elasticsearch CCR to migrate data across data centers](https://www.elastic.co/cn/blog/cross-datacenter-replication-with-elasticsearch-cross-cluster-replication).

3.  Insert a data record into the remote cluster and check whether the data record is migrated to the local cluster in real time.

    ```
    POST myindex/_doc/
    {
      "name":"Pony",
      "age":50
    }
    ```

    Query the inserted data record in the local cluster immediately. The following figure shows the data record.

    ![Verify real-time data migration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0301852061/p135046.png)

    The preceding figure shows that the CCR feature can implement real-time migration of incremental data.

    **Note:** You can also call the API operations of the CCR feature to perform cross-cluster replication operations. For more information, see [Cross-cluster replication APIs](https://www.elastic.co/guide/en/elasticsearch/reference/master/ccr-apis.html#ccr-apis).


## FAQ

Q: I can use port 9300 to [add a remote cluster](#section_op9_634_jh1). Why is only port 9200 accessible when I use a domain name to access an Elasticsearch cluster?

A: Port 9300 is an open port. However, when you access a cluster over the Internet, Server Load Balancer \(SLB\) enables only port 9200 during port verification for security purposes. This will be adjusted in the future.

