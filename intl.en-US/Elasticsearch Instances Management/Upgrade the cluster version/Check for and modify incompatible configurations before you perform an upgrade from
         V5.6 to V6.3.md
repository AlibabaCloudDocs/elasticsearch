# Check for and modify incompatible configurations before you perform an upgrade from V5.6 to V6.3

A later version may be incompatible with some configurations in an earlier version. If you do not modify these configurations before a version upgrade, your services may be affected after the upgrade. Therefore, before the upgrade, you must check for incompatible configurations and modify the configurations as required. This topic describes the manual checks that you must perform and the automatic check items before an upgrade from V5.6.16 to V6.3.2. It also provides the methods that can be used to modify the incompatible configurations.

When you check for incompatible configurations, take note of the following points:

-   Before a version upgrade, a precheck is performed. You must identify incompatible configurations based on the precheck results and modify the configurations as required. For more information, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade the cluster version/Upgrade the version of a cluster.md).
-   The commands provided in this topic can be executed in the Kibana console. For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

## Perform manual checks

1.  Split each multi-type index on the Elasticsearch V5.X cluster into multiple single-type indexes.

    Elasticsearch clusters of V6.X or later do not support multi-type indexes. If the V5.X cluster contains multi-type indexes, you can write data to the indexes but cannot create multi-type indexes after the cluster version is upgraded to V6.X. If you create multi-type indexes, errors are reported. Therefore, before the upgrade, we recommend that you split each multi-type index into single-type indexes. For more information about how to split a multi-type index, see [Use the reindex operation to migrate data in a multi-type index]().

2.  Check whether the cluster contains indexes that are in the close state.

    ```
    GET _cat/indices?v
    ```

    ![Check for indexes that are in the close state](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8483836061/p178672.png)

    -   Yes: Run the following command to open the indexes that are in the close state:

        ```
        POST test/_open
        ```

    -   No: Go to the next step.
3.  Check whether the cross-cluster search feature is enabled for the cluster.

    ```
    GET _cluster/settings
    ```

    -   Yes: Disable the feature. You can enable it after the upgrade.

        ```
        PUT _cluster/settings
        {
          "persistent": {
            "search.remote.*": null
          },
          "transient": {
            "search.remote.*": null
          }
        }
        ```

        **Note:** The search.remote parameter is used to configure the cross-cluster search feature in V5.X, whereas the cluster.remote parameter is used in V6.X.

    -   No: No actions are required.

## Automatic check items

Before you upgrade the version of your cluster, you must click **Precheck** to check the cluster. For more information, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade the cluster version/Upgrade the version of a cluster.md). Then, the system performs a compatibility check based on the following table.

|No.|Configuration level|Category|Parameter|
|---|-------------------|--------|---------|
|1|Cluster|Snapshot settings|cluster.routing.allocation.snapshot.relocation\_enabled|
|2|Storage throttling settings|indices.store.throttle.type and indices.store.throttle.max\_bytes\_per\_sec|
|3|Index|Similarity settings|index.similarity.base|
|4|Shadow replica settings|index.shared\_filesystem and index.shadow\_replicas|
|5|Index storage settings|index.store.type|
|6|Storage throttling settings|index.store.throttle.type and index.store.throttle.max\_bytes\_per\_sec|
|7|include\_in\_all setting in the mappings configuration of an index|include\_in\_all**Note:** The indexes that are created before an upgrade from V5.X to V6.X and have this parameter configured can still be used after the upgrade. The indexes that are created after the upgrade do not support this parameter. |
|8|Version settings for index creation|index.version.created**Note:** This parameter specifies that indexes cannot be upgraded across major versions. For example, you cannot directly upgrade the indexes created in V5.X to V7.X. Before an upgrade from V5.X to V7.X, you must call the reindex operation to migrate data in the indexes to a V7.X cluster. Then, delete the indexes from the V5.X cluster and upgrade the version of the V5.X cluster. |
|9|Index template|Similarity settings|index.similarity.base|
|10|Shadow replica settings|index.shared\_filesystem and index.shadow\_replicas|
|11|Index storage settings|index.store.type|
|12|Storage throttling settings|index.store.throttle.type and index.store.throttle.max\_bytes\_per\_sec|
|13|include\_in\_all setting in the mappings configuration of the index template|include\_in\_all|
|14|\_all setting in the mappings configuration of the index template|\_all|
|15|Type settings in the mappings configuration of the index template|None**Note:** Check whether the mappings configuration in the index template contains multiple type settings. |

**Note:** The preceding check items are at the CRITICAL level. If a CRITICAL check item is reported, the cluster fails the compatibility check, and its version cannot be upgraded. A later version is incompatible with the configurations indicated by CRITICAL check items. If a WARNING check item is reported, the cluster fails the compatibility check, but its version can still be upgraded. The configurations indicated by WARNING check items are ignored after the upgrade.

## Modify incompatible configurations

This section provides the methods that are used to modify incompatible configurations.

-   Cluster-level incompatible configurations

    For cluster-level incompatible configurations, you can disable the configurations.

    |Configuration category|Command to disable the configuration|
    |----------------------|------------------------------------|
    |Snapshot settings|    ```
PUT _cluster/settings
{
  "persistent": {
    "cluster.routing.allocation.snapshot.relocation_enabled": null
  },
  "transient": {
    "cluster.routing.allocation.snapshot.relocation_enabled": null
  }
}
    ``` |
    |Storage throttling settings|    ```
PUT _cluster/settings
{
  "persistent": {
    "indices.store.throttle.type": null,
    "indices.store.throttle.max_bytes_per_sec": null
  },
  "transient": {
    "indices.store.throttle.type": null,
    "indices.store.throttle.max_bytes_per_sec": null
  }
}
    ``` |

-   Index-level incompatible configurations

    For index-level incompatible configurations, you can disable the configurations.

    |Configuration category|Command to disable the configuration|Additional information|
    |----------------------|------------------------------------|----------------------|
    |Similarity settings|    ```
PUT test_index/_settings
{
   "index.similarity.base.*": null
}
    ```

|These configurations can be modified only after indexes are closed. You cannot read data from or write data to closed indexes. After the modifications, you can open the indexes. The following example shows how to open and close the test\_index index:    -   Close the index

        ```
POST test_index/_close
        ```

    -   Open the index

        ```
POST test_index/_open
        ``` |
    |Shadow replica settings|    ```
PUT test_index/_settings
{
    "index.shared_filesystem": null,
    "index.shadow_replicas": null
}
    ``` |
    |Index storage settings|    ```
PUT test_index/_settings
{
   "index.store.type": null
}
    ``` |
    |Storage throttling settings|    ```
PUT test_index/_settings
{
  "settings": {
    "index.store.throttle.type": null,
    "index.store.throttle.max_bytes_per_sec": null
  }
}
    ```

|None.|

    **Note:** Indexes that have the include\_in\_all parameter configured can still be used in the later version. You do not need to modify this parameter.

-   Index template-level incompatible configurations

    The following example shows how to modify configurations in the test\_template index template:

    1.  Run the `GET _template/test_template` command to query the test\_template index template.

        The query result shows that the index template contains the following incompatible configurations: index storage settings, \_all, and include\_in\_all.

        ```
        {
         "test_template": {
           "order": 0,
           "template": "test_*",
           "settings": {
             "index": {
               "store": {
                 "throttle": {
                   "max_bytes_per_sec": "100m"
                 }
               }
             }
           },
           "mappings": {
             "test_type": {
               "_all": {
                 "enabled": true
               },
               "properties": {
                 "test_field": {
                   "type": "text",
                   "include_in_all": true
                 }
               }
             }
           },
           "aliases": {}
         }
        }
        ```

    2.  Delete the incompatible configurations and run the `PUT _template/test_template` command to update the index template.

        ```
        PUT _template/test_template
        {
           "order": 0,
           "template": "test_*",
           "settings": {
           },
           "mappings": {
             "test_type": {
               "properties": {
                 "test_field": {
                   "type": "text"
                 }
               }
             }
           },
           "aliases": {}
        }
        ```


