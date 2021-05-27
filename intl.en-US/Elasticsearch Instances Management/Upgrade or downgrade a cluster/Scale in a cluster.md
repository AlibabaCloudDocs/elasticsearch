---
keyword: [remove Elasticsearch data nodes, migrate Elasticsearch data, roll back Elasticsearch data migration]
---

# Scale in a cluster

If your business is in the off-peak hours of traffic or the volume of data stored on your cluster decreases, you can remove data nodes from your cluster to scale in it. This topic describes how to remove data nodes from a cluster to scale in it.

## Prerequisites

[Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) to check whether your cluster stores indexes in the close state. If your cluster stores such indexes, you must open or delete the indexes. Otherwise, the upgrade fails.

-   Run the following command to view the statuses of indexes:

    ```
    GET /_cat/indices?v
    ```

    ![View the statuses of indexes](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5135141261/p244657.png)

-   Run the following command to enable the index in the close state:

    ```
    POST /<index_name>/_open
    ```

-   Run the following command to delete the index in the close state:

    **Warning:** Proceed with caution because the deleted indexes cannot be recovered.

    ```
    DELETE /<index_name>
    ```


## Precautions

-   After you remove data nodes from a cluster, the system restarts the cluster. The time required for the restart depends on the size, data volume, and load of your cluster. We recommend that you remove data nodes during off-peak hours.
-   In most cases, if the indexes of your cluster have replica shards and the load of your cluster is normal, your cluster can still provide services during a restart. The following items indicate that the load of a cluster is normal: The CPU utilization of the cluster is about 60%, the heap memory usage of the cluster is about 50%, and the value of NodeLoad\_1m is less than the number of vCPUs for the cluster.
-   If the indexes of your cluster do not have replica shards, the load of the cluster is excessively high, and large amounts of data are written to or queried in your cluster, the access to your cluster may time out during data node removal. We recommend that you configure a retry mechanism for your client before you remove data nodes. This reduces the impact on your business.

## Remove data nodes

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  Navigate to the desired cluster.

    1.  In the top navigation bar, select a resource group and a region.

    2.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the **Elasticsearch Clusters** page, find the desired cluster and click its ID.

4.  In the lower-right corner of the **Basic Information** page, choose **Configuration Update** \> **Remove Data Nodes**.

5.  In the **Remove Data Nodes** section of the page that appears, set **Node Type**.

6.  Select the data nodes that you want to remove.

    **Note:** The number of reserved nodes must be greater than two and greater than half of the existing nodes.

7.  Migrate data.

    For security purposes, make sure that the data nodes you want to remove store no data. If these data nodes store data, the system prompts you to migrate the data. After the data is migrated, no index data is stored on the data nodes, and no index data is written to them.

    1.  Click **Data Migration Tool** in the message that appears.

        ![Data migration tool](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3467819951/p45748.png)

    2.  In the **Migrate Data** dialog box, select a data migration method.

        |Parameter|Description|
        |---------|-----------|
        |**Smart Migration**|The system automatically selects data nodes for data migration.|
        |**Custom Migration**|You must manually select the data nodes whose data you want to migrate.|

    3.  Read and agree to the terms of data migration, and click **OK**.

    4.  Click **OK**.

        Then, the system restarts the cluster. During the restart, you can view the data migration progress in the Tasks dialog box. After the cluster is restarted, the data stored on the data nodes that you want to remove is migrated.

        ![Tasks dialog box](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5343802261/p93945.png)

        **Note:** During data migration, you can click **Pause** in the **Tasks** dialog box to pause the migration.

8.  In the lower-right corner of the **Basic Information** page, choose **Configuration Update** \> **Remove Data Nodes** again.

9.  In the **Remove Data Nodes** section of the page that appears, select the data nodes whose data is migrated and click **OK**.

    Then, the system restarts the cluster. During the restart, you can view the data migration progress in the Tasks dialog box. After the cluster is restarted, the data nodes are removed from the cluster.

    ![Scale-in process](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3467819951/p45785.png)


## Roll back data migration

Data migration is time-consuming. Cluster status changes or data modifications may result in a data migration failure. You can view detailed information in the Tasks dialog box. To roll back data migration, perform the following steps:

1.  Log on to the Kibana console of your cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to obtain the IP addresses of the data nodes whose data is migrated:

    ```
    GET _cluster/settings
    ```

    If the running of the command is successful, the following result is returned:

    ```
    {
      "transient": {
        "cluster": {
          "routing": {
            "allocation": {
              "exclude": {
                "_ip": "192.168.xx.xx,192.168.xx.xx,192.168.xx.xx"
              }
            }
          }
        }
      }
    }                        
    ```

4.  Roll back data.

    -   Roll back the data on some data nodes. Use the exclude parameter to exclude the data nodes whose data you do not want to roll back.

        ```
        PUT _cluster/settings
        {
          "transient": {
            "cluster": {
              "routing": {
                "allocation": {
                  "exclude": {
                    "_ip": "192.168.xx.xx,192.168.xx.xx"
                  }
                }
              }
            }
          }
        }
        ```

    -   Roll back the data on all data nodes.

        ```
        PUT _cluster/settings
        {
          "transient": {
            "cluster": {
              "routing": {
                "allocation": {
                  "exclude": {
                    "_ip": null
                  }
                }
              }
            }
          }
        }                            
        ```

5.  Run the following command to check whether the data is rolled back:

    ```
    GET _cluster/settings
    ```

    If the IP addresses of the data nodes whose data is rolled back are not contained in the command output, the rollback is successful. You can also check the rollback progress based on whether shards are reallocated to the data nodes.

    **Note:** To check the status of a data migration or rollback task, run the `GET _cat/shards?v` command.


## FAQ

-   What do I do if the "This operation may cause a shard distribution error or insufficient storage, CPU, or memory resources." message appears?

    Cause

    -   Insufficient resources

        After data nodes are removed, the cluster does not have sufficient resources to store system data or handle workloads. The resources include disk space, memory, and CPU resources.

    -   Shard allocation errors

        Elasticsearch is based on Lucene principles. It does not migrate two or more replica shards of the same index on a data node to the same data node. In this case, after data nodes are removed, the number of replica shards in a cluster may be greater than or equal to the number of data nodes. This results in shard allocation errors.

    Solution

    -   Insufficient resources

        Run the `GET _cat/indices?v` command to check whether the resource usage of your cluster, such as disk usage, is greater than the threshold. You must make sure that the cluster has sufficient resources to store data and process requests. If these requirements are not met, upgrade the configuration of the cluster. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).

    -   Shard allocation errors

        Run the `GET _cat/indices?v` command to check whether the number of replica shards in the cluster is less than the number of data nodes after data nodes are removed. If this requirement is not met, change the number of replica shards. For more information, see [Index Templates](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/indices-templates.html). The following sample code demonstrates how to change the number of replica shards to 2 in the index template:

        ```
        PUT _template/template_1
        {
          "template": "*",
          "settings": {
            "number_of_replicas": 2
          }
        }  
        ```

-   What do I do if the "The cluster is running tasks or in an error status. Try again later." message appears?

    Solution: Run the `GET _cluster/health` command to check the status of the cluster or go to the pages below **Intelligent Maintenance** to view the cause.

-   What do I do if the "The nodes in the cluster contain data. You must migrate the data first." message appears?

    Solution: Migrate data. For more information, see [Remove data nodes](#section_kga_q78_5jk).

-   What do I do if the "The number of nodes that you reserve must be more than two and more than half of the existing nodes." message appears?

    Cause: The number of reserved data nodes does not meet requirements. To ensure cluster reliability, the number of data nodes reserved during data node removal must be greater than two. To ensure cluster stability, the number of data nodes selected for removal or data migration must be less than or equal to half of the existing data nodes.

    Solution: If the preceding requirements are not met, re-select the data nodes or upgrade the configuration of the cluster. For more information about how to upgrade the configuration of a cluster, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).

-   What do I do if the "The current Elasticsearch cluster configuration does not support this operation. Check the Elasticsearch cluster configuration first." message appears?

    Solution: Run the `GET _cluster/settings` command to view the cluster configuration. Then, check whether the cluster configuration contains the settings that do not allow data allocation.

-   What do I do if data nodes fail to be removed or data fails to be migrated due to the `auto_expand_replicas` index setting?

    Cause: Some users may use the access control feature provided by the X-Pack plug-in. In earlier Elasticsearch versions, this feature applies the `"index.auto_expand_replicas" : "0-all"` setting to the .security index by default. This causes errors when you migrate data or remove data nodes.

    Solution:

    1.  Query index settings.

        ```
        GET .security/_settings
        ```

        The following result is returned:

        ```
        {
          ".security-6" : {
            "settings" : {
              "index" : {
                "number_of_shards" : "1",
                "auto_expand_replicas" : "0-all",
                "provided_name" : ".security-6",
                "format" : "6",
                "creation_date" : "1555142250367",
                "priority" : "1000",
                "number_of_replicas" : "9",
                "uuid" : "9t2hotc7S5OpPuKEIJ****",
                "version" : {
                  "created" : "6070099"
                }
              }
            }
          }
        }
        ```

    2.  Use one of the following methods to modify the auto\_expand\_replicas index setting:
        -   Method 1

            ```
            PUT .security/_settings
            {
              "index" : {
                "auto_expand_replicas" : "0-1"
              }
            }
            ```

        -   Method 2

            ```
            PUT .security/_settings
            {
              "index" : {
                "auto_expand_replicas" : "false",
                "number_of_replicas" : "1"
              }
            }
            ```

            **Note:** Set `number_of_replicas` based on your business requirements. Make sure that the number of replica shards enabled for each index is greater than or equal to one but no more than the number of available data nodes.


