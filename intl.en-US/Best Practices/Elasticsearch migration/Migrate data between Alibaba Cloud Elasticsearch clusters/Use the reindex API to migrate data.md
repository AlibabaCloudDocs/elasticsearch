# Use the reindex API to migrate data

You can use the reindex API to migrate data from an Elasticsearch cluster of an earlier version to an Elasticsearch cluster of the current version. You can also use this API to replicate data from an existing index in a local cluster to a new index in a remote cluster. This topic describes the procedure in detail.

## Precautions

The adjustment made to the Alibaba Cloud Elasticsearch network architecture has the following impacts on clusters:

-   Clusters created after October 2020 do not support the X-Pack Watcher and Lightweight Directory Access Protocol \(LDAP\) authentication features.
-   You cannot reindex, search for, and replicate data between a cluster created before October 2020 and a cluster created after October 2020. If you want to perform these operations between them, make sure that the clusters are under the same network architecture.

**Note:** The network architecture in the China \(Zhangjiakou\) region and the regions outside China was adjusted before October 2020. If you want to perform the preceding operations between a cluster created before October 2020 and that created after October 2020 in such a region, [submit a ticket](https://workorder-intl.console.aliyun.com/console.htm) to contact technical support personnel to check whether the network architecture supports the operations.

## Prerequisites

-   Two Alibaba Cloud Elasticsearch clusters are created. One is used as a local cluster, and the other is used as a remote cluster.

    For more information, see [t134282.md\#](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md). The two clusters must belong to the same virtual private cloud \(VPC\) and vSwitch. This topic uses an Elasticsearch V6.7.0 cluster as the local cluster and an Elasticsearch V6.3.2 cluster as the remote cluster.

-   Test data is prepared.
    -   Local cluster

        Create a destination index in the local cluster.

        ```
        PUT dest
        {
          "settings": {
            "number_of_shards": 5,
            "number_of_replicas": 1
          }
        }
        ```

    -   Remote cluster

        Prepare the data that you want to migrate. This topic uses the data in the "Quick start" topic as the test data. For more information, see [Quick start](/intl.en-US/Elasticsearch Instances Management/Quick start.md).

        **Note:** If you use a cluster of V7.0 or later, you must set the index type to \_doc.


## Background information

You can use the reindex API to perform the following operations:

-   Migrate data between Elasticsearch clusters.
-   Reindex data in an index whose shards are inappropriately configured. For example, the data volume is large, but only a few shards are configured for the index. This slows down data write operations.
-   Replicate data in an index if the index stores large volumes of data and you want to change the mapping configuration of the index. This method requires only a short period of time. You can also import the data into a new index. However, this method is time-consuming.

    **Note:** After you define the mapping configuration for an index in an Elasticsearch cluster and import data into the index, you cannot change the mapping configuration.


## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  Navigate to the desired cluster.

    1.  In the top navigation bar, select a resource group and a region.

    2.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the **Elasticsearch Clusters** page, find the desired cluster and click its ID.

4.  Configure a reindex whitelist for the local cluster.

    1.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

    2.  On the page that appears, click **Modify Configuration** on the right side of **YML Configuration**.

    3.  In the **Other Configurations** field of the **YML File Configuration** panel, configure a reindex whitelist.

        For more information about how to configure a reindex whitelist, see [Configure a remote reindex whitelist](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md).

        -   For a single-zone Elasticsearch cluster, specify the whitelist in the format of <Endpoint of the cluster\>:9200.

            ![Single-zone cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7740628061/p135190.png)

            ```
            reindex.remote.whitelist: ["es-cn-09k1rgid9000g****.elasticsearch.aliyuncs.com:9200"]
            ```

        -   For a multi-zone Elasticsearch cluster, the whitelist must contain the IP addresses of all data nodes in the cluster and the port number of the cluster.

            ![Configuration example of a multi-zone cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5802852061/p135853.png)

            ```
            reindex.remote.whitelist: ["10.0.xx.xx:9200","10.0.xx.xx:9200","10.0.xx.xx:9200","10.15.xx.xx:9200","10.15.xx.xx:9200","10.15.xx.xx:9200"]
            ```

            **Note:** You can obtain the IP addresses of all data nodes in a cluster from the **Node Visualization** tab on the **Basic Information** page of the cluster. For more information, see [View the basic information of nodes](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the cluster status and node information.md).

    4.  Select **This operation will restart the cluster. Continue?** and click **OK**.

5.  After the cluster is restarted, configure the local cluster to connect it to the remote cluster.

    In the local cluster, add the remote cluster. For more information, see [Connect Elasticsearch clusters](/intl.en-US/Elasticsearch Instances Management/Security/Connect Elasticsearch clusters.md).

    ![Connect clusters](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5802852061/p135740.png)

6.  In the local cluster, use the reindex API to reindex data.

    [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) of the local cluster and run the following command to reindex data:

    ```
    POST _reindex
    {
      "source": {
        "remote": {
          "host": "http://es-cn-09k1rgid9000g****.elasticsearch.aliyuncs.com:9200",
          "username": "elastic",
          "password": "your_password"
        },
        "index": "product_info",
        "query": {
          "match": {
            "productName": "Wealth management"
          }
        }
      },
      "dest": {
        "index": "dest"
      }
    }
    ```

    |Part|Parameter|Description|
    |----|---------|-----------|
    |source|host|The URL that is used to access the remote cluster. The URL must contain the protocol, domain name, and port number, such as https://otherhost:9200.     -   For a single-zone cluster, set host to a value in the format of http://<Domain name of the cluster\>:9200.

**Note:** You can obtain the domain name from the Basic Information page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).

    -   For a multi-zone cluster, set host to a value in the format of http://<IP address of a data node in the cluster\>:9200. |
    |username|Optional. If the remote cluster needs to use basic access authentication, specify this parameter. The default username of Alibaba Cloud Elasticsearch clusters is elastic. **Note:**

    -   For security purposes, basic access authentication must be implemented over HTTPS. Otherwise, the related password is transmitted in plaintext.
    -   For Alibaba Cloud Elasticsearch clusters, you can use HTTPS for host only after you enable this protocol for the cluster. |
    |password|The password that corresponds to the username. The password is specified when you create the cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|
    |index|The source index in the remote cluster.|
    |query|Specifies the data that you want to migrate. For more information, see [Reindex API](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/docs-reindex.html?spm=a2c4g.11186623.2.16.4cd96ac0AD7xUw).|
    |dest|index|The destination index in the local cluster.|

    **Note:** When you reindex data from a remote cluster, manual slicing and automatic slicing are not supported. For more information, see [Manual slicing](https://github.com/elastic/elasticsearch/blob/5.6/docs/reference/docs/reindex.asciidoc?spm=a2c4g.11186623.2.17.4cd96ac0AD7xUw#docs-reindex-manual-slice) and [Automatic slicing](https://github.com/elastic/elasticsearch/blob/5.6/docs/reference/docs/reindex.asciidoc?spm=a2c4g.11186623.2.18.4cd96ac0AD7xUw#docs-reindex-automatic-slice).

    If the running of the command is successful, the following result is returned:

    ```
    {
      "took" : 51,
      "timed_out" : false,
      "total" : 2,
      "updated" : 2,
      "created" : 0,
      "deleted" : 0,
      "batches" : 1,
      "version_conflicts" : 0,
      "noops" : 0,
      "retries" : {
        "bulk" : 0,
        "search" : 0
      },
      "throttled_millis" : 0,
      "requests_per_second" : -1.0,
      "throttled_until_millis" : 0,
      "failures" : [ ]
    }
    ```

7.  Run the following command to view the migrated data:

    ```
    GET dest/_search
    ```

    The following figures show the command outputs.

    -   Single-zone cluster

        ![View the migrated data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5802852061/p135751.png)

    -   Multi-zone cluster

        ![View the migrated data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5802852061/p135865.png)


## Summary

When you use the reindex API to migrate data from a cluster, the configurations for a single-zone cluster are similar to those for a multi-zone cluster. The following table lists differences.

|Cluster type|Configuration of the reindex whitelist|Configuration of the host parameter|
|------------|--------------------------------------|-----------------------------------|
|Single-zone cluster|Domain name of the cluster:9200|https://Domain name of the cluster:9200|
|Multi-zone cluster|Combination of the IP addresses of all data nodes in the cluster and the port number|https://IP address of a data node in the cluster:9200|

## Additional information

When you use the reindex API to reindex data, you can specify a batch size and timeout periods.

-   Batch size

    A remote Elasticsearch cluster uses a heap to cache index data. The default batch size is 100 MB. If an index in the remote cluster contains large documents, you must adjust the batch size to a smaller value.

    In the following example, size is set to 10.

    ```
    POST _reindex
    {
      "source": {
        "remote": {
          "host": "http://otherhost:9200"
        },
        "index": "source",
        "size": 10,
        "query": {
          "match": {
            "test": "data"
          }
        }
      },
      "dest": {
        "index": "dest"
      }
    }
    ```

-   Timeout values

    Use socket\_timeout to set a timeout value for socket reads. The default value is 30s. Use connect\_timeout to set a timeout value for connections. The default value is 1s.

    In the following example, the timeout value for socket reads is set to 1 minute, and that for connections is set to 10 seconds.

    ```
    POST _reindex
    {
      "source": {
        "remote": {
          "host": "http://otherhost:9200",
          "socket_timeout": "1m",
          "connect_timeout": "10s"
        },
        "index": "source",
        "query": {
          "match": {
            "test": "data"
          }
        }
      },
      "dest": {
        "index": "dest"
      }
    }
    ```


