---
keyword: [Elasticsearch YML file configuration, Auto Indexing, Index Deletion, Audit Log Indexing, Watcher]
---

# Modify the YML file configuration

This topic describes how to modify the YML file configuration of your Alibaba Cloud Elasticsearch cluster. You can enable Auto Indexing, Index Deletion, Audit Log Indexing, or Watcher. You can also specify Other Configurations.

## Procedure

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

5.  On the **Cluster Configuration** page, click **Modify Configuration** on the right side of **YML Configuration**.

6.  In the **YML File Configuration** pane, configure the following parameters.

    ![Elasticsearch YML file configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8467819951/p40138.png)

    |Parameter|Description|
    |---------|-----------|
    |**Auto Indexing**|Specifies whether to automatically create an index when a new document is uploaded to your Elasticsearch cluster but no index has been created. We recommend that you disable Auto Indexing because indexes created by this feature may not meet your business requirements. This parameter corresponds to the `action.auto_create_index` configuration item in the YML file. The default value of this configuration item is `false`. |
    |**Index Deletion**|Specifies whether to specify the index name when you delete an index. If you select **Allow Wildcards**, you can use wildcards to delete multiple indexes at a time. Deleted indexes cannot be restored. Exercise caution when you configure this parameter. This parameter corresponds to the `action.destructive_requires_name` configuration item in the YML file. The default value of this configuration item is `true`. |
    |**Audit Log Indexing**|If you enable Audit Log Indexing, index logs are generated when you create, delete, modify, or search for an index in your Elasticsearch cluster. These logs consume disk space and affect cluster performance. Therefore, we recommend that you disable Audit Log Indexing. Exercise caution when you configure this parameter. **Note:** This parameter is unavailable for Elasticsearch V7.4.0 clusters.

This parameter corresponds to the `xpack.security.audit.enabled` configuration item in the YML. The default value of this configuration item is `false`. |
    |**Watcher**|If you enable Watcher, you can use the X-Pack Watcher feature. Make sure that you clear the `.watcher-history*` index on a regular basis to free up disk space. This parameter corresponds to the `xpack.watcher.enabled` configuration item in the YML file. The default value of this configuration item is `false`. |
    |**Other Configurations**|The following content lists some of the supported configuration items. Unless otherwise specified, these items are available for Elasticsearch V5.X, V6.X, and V7.X.     -   [Configure CORS](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure YML/Configure CORS.md)
        -   `http.cors.enabled`
        -   `http.cors.allow-origin`
        -   `http.cors.max-age`
        -   `http.cors.allow-methods`
        -   `http.cors.allow-headers`
        -   `http.cors.allow-credentials`
    -   [Recreate indexes by calling the Reindex operation](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure YML/Recreate indexes by calling the Reindex operation.md)

`reindex.remote.whitelist`

    -   [Configure the audit log indexing feature](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure YML/Configure the audit log indexing feature.md)
        -   `xpack.security.audit.enabled`
        -   `xpack.security.audit.index.bulk_size`
        -   `xpack.security.audit.index.flush_interval`
        -   `xpack.security.audit.index.rollover`
        -   `xpack.security.audit.index.events.include`
        -   `xpack.security.audit.index.events.exclude`
        -   `xpack.security.audit.index.events.emit_request_body`
    -   [Configure queue sizes](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure YML/Configure queue sizes.md)
        -   `thread_pool.bulk.queue_size` \(available for Elasticsearch V5.X\)
        -   `thread_pool.write.queue_size` \(available for Elasticsearch V6.X and V7.X\)
        -   `thread_pool.search.queue_size`
    -   Configure a custom SQL plug-in

`xpack.sql.enabled`

By default, Elasticsearch uses the built-in SQL plug-in provided by X-Pack. To upload a custom SQL plug-in, set `xpack.sql.enabled` to `false`. |

    **Warning:** After you modify the **YML file configuration** of your Elasticsearch cluster, the system performs a rolling restart on the cluster for the modifications to take effect. If replica shards are configured for the indexes in your cluster, the rolling restart does not affect your business. Otherwise, the rolling restart may affect your business. Therefore, make sure that you want to proceed with the modifications. We recommend that you modify the YML file configuration during off-peak hours.

7.  Scroll down to the lower part of the pane, select **This operation will restart the cluster. Continue?**, and click **OK**.

    The system restarts the Elasticsearch cluster. You can view the restart progress in the [Tasks](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the progress of a cluster task.md) dialog box. After the cluster is restarted, the YML file configuration is updated.


