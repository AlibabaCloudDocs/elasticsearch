---
keyword: [Elasticsearch reindex, recreate indexes]
---

# Configure a remote reindex whitelist for an Elasticsearch cluster

Before you call the reindex operation to migrate index data in a remote Elasticsearch cluster to the current Elasticsearch cluster, you must configure a remote reindex whitelist for the current Elasticsearch cluster. This topic describes how to configure a remote reindex whitelist.

## Configuration description

In the **Other Configurations** field, specify the `reindex.remote.whitelist` parameter to add the endpoint of the remote cluster to the remote reindex whitelist of the current cluster. For more information, see [Modify the YML file configuration](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Modify the YML file configuration.md).

In the whitelist, specify a remote cluster in the `Host address`:`Port number` format. If you want to specify multiple remote clusters, separate them with commas \(,\), such as `otherhost:9200,another:9200,127.0.10.**:9200,localhost:**`. The whitelist does not identify protocols. Therefore, you can use only host addresses and port numbers to specify remote clusters.

When you configure a remote reindex whitelist, if the remote cluster is a single-zone Alibaba Cloud Elasticsearch cluster, specify this cluster in the `<Endpoint of the cluster>:9200` format. If the remote cluster is a multi-zone Alibaba Cloud Elasticsearch cluster, specify this cluster by using the IP addresses of all data nodes in the cluster and the port number of the cluster. Configuration examples:

-   Single-zone cluster

    ![Single-zone cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7740628061/p135190.png)

    ```
    reindex.remote.whitelist: ["es-cn-09k1rgid9000g****.elasticsearch.aliyuncs.com:9200"]
    ```

-   Multi-zone cluster

    ![Multi-zone cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7740628061/p135207.png)

    ```
    reindex.remote.whitelist: ["10.0.xx.xx:9200","10.0.xx.xx:9200","10.0.xx.xx:9200","10.15.xx.xx:9200","10.15.xx.xx:9200","10.15.xx.xx:9200"]
    ```

    **Note:** After the remote reindex whitelist is configured, you can call the reindex operation to reindex data. For more information, see [Call the reindex operation to migrate data](/intl.en-US/Best Practices/Elasticsearch migration/Migrate data between Alibaba Cloud Elasticsearch clusters/Call the reindex operation to migrate data.md).


