---
keyword: [Elasticsearch YML file configuration, Auto Indexing, Index Deletion, Audit Log Indexing, Watcher]
---

# Configure the YML file

In the YML Configuration section of the Cluster Configuration page of your Alibaba Cloud Elasticsearch cluster, you can enable the Auto Indexing, Audit Log Indexing, or Watcher feature. You can also specify Index Deletion and Other Configurations. This topic describes how to configure the following items: parameters in the YML file of the cluster, cross-origin resource sharing \(CORS\), a remote reindex whitelist, the Audit Log Indexing feature, and queue sizes.

## Precautions

Due to the adjustment made to the Alibaba Cloud Elasticsearch network architecture, clusters created after October 2020 do not support the X-Pack Watcher and LDAP authentication features. You cannot reindex, search for, and replicate data between a cluster created before October 2020 and a cluster created after October 2020. You can perform the operations only between clusters created before October 2020 or between clusters created after October 2020. The features will be available soon.

## Configure the parameters in the YML file

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  Navigate to the desired cluster.

    1.  In the top navigation bar, select a resource group and a region.

    2.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the **Elasticsearch Clusters** page, find the desired cluster and click its ID.

4.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

5.  On the **Cluster Configuration** page, click **Modify Configuration** on the right side of **YML Configuration**.

6.  In the **YML File Configuration** panel, configure the following parameters.

    ![Elasticsearch YML file configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5421416161/p40138.png)

    |Parameter|Description|
    |---------|-----------|
    |**Auto Indexing**|Specifies whether to automatically create an index when a new document is uploaded to your Elasticsearch cluster but no index exists. We recommend that you disable Auto Indexing because indexes created by this feature may not meet your business requirements. This parameter corresponds to the action.auto\_create\_index configuration item in the YML file. The default value of this configuration item is false. |
    |**Index Deletion**|Specifies whether to specify the index name when you delete an index. If you set this parameter to **Allow Wildcards**, you can use wildcards to delete multiple indexes at a time. Deleted indexes cannot be recovered. Exercise caution when you configure this parameter. This parameter corresponds to the action.destructive\_requires\_name configuration item in the YML file. The default value of this configuration item is true. |
    |**Audit Log Indexing**|If you enable Audit Log Indexing, the system generates audit logs when you create, delete, modify, or search for an index in your Elasticsearch cluster. These logs consume disk space and affect cluster performance. Therefore, we recommend that you disable Audit Log Indexing and exercise caution when you configure this parameter. For more information, see [Configure the Audit Log Indexing feature](#section_51s_io4_0lq). **Note:** This parameter is unavailable for Elasticsearch clusters of V7.0 or later.

This parameter corresponds to the xpack.security.audit.enabled configuration item in the YML file. The default value of this configuration item is false. |
    |**Watcher**|If you enable Watcher, you can use the X-Pack Watcher feature. You must clear the .watcher-history\* index on a regular basis to free up disk space. This parameter corresponds to the xpack.watcher.enabled configuration item in the YML file. The default value of this configuration item is false. |
    |**Other Configurations**|The following description provides some of the supported configuration items. Unless otherwise specified, these items are available for Elasticsearch V5.X, V6.X, and V7.X.     -   [Configure CORS](#section_qq3_vkg_2fb)
        -   http.cors.enabled
        -   http.cors.allow-origin
        -   http.cors.max-age
        -   http.cors.allow-methods
        -   http.cors.allow-headers
        -   http.cors.allow-credentials
    -   [Configure a remote reindex whitelist](#section_wb5_fwy_qld)

reindex.remote.whitelist

    -   [Configure the Audit Log Indexing feature](#section_51s_io4_0lq)

**Note:** The Audit Log Indexing feature is not supported in Elasticsearch V7.0 and later.

        -   xpack.security.audit.enabled
        -   xpack.security.audit.index.bulk\_size
        -   xpack.security.audit.index.flush\_interval
        -   xpack.security.audit.index.rollover
        -   xpack.security.audit.index.events.include
        -   xpack.security.audit.index.events.exclude
        -   xpack.security.audit.index.events.emit\_request\_body
    -   [Configure queue sizes](#section_s6x_nx8_gyy)
        -   thread\_pool.bulk.queue\_size \(available for Elasticsearch V5.X\)
        -   thread\_pool.write.queue\_size \(available for Elasticsearch V6.X and V7.X\)
        -   thread\_pool.search.queue\_size
    -   Configure a custom SQL plug-in

xpack.sql.enabled

By default, Elasticsearch uses the built-in SQL plug-in provided by X-Pack. To upload a custom SQL plug-in, set xpack.sql.enabled to false. |

    **Warning:**

    -   Before you configure the YML file, you must make sure that the cluster is in a normal state. After you configure the YML file, the system restarts the cluster. The time required for the restart depends on the size, data volume, and load of the cluster. We recommend that you configure the YML file during off-peak hours.
    -   In most cases, if the indexes of your cluster have replica shards and the load of your cluster is normal, your cluster can still provide services during a cluster modification. The following items indicate that the load of a cluster is normal: The CPU utilization of the cluster is about 60%, the heap memory usage of the cluster is about 50%, and the value of NodeLoad\_1m is less than the number of vCPUs for the cluster.

    -   If the indexes of your cluster do not have replica shards, the load of the cluster is excessively high, and large amounts of data are written to or queried in your cluster, access timeouts may occur during a cluster modification. We recommend that you configure an access retry mechanism for your client before you perform a cluster modification. This reduces the impact on your business.

7.  Scroll down to the lower part of the panel, select **This operation will restart the cluster. Continue?**, and then click **OK**.

    Then, the system restarts the Elasticsearch cluster. You can view the restart progress in the [Tasks](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the progress of a cluster task.md) dialog box. After the cluster is restarted, the configurations in the YML file are updated.


## Configure CORS

You can configure CORS to allow browsers on other origins to access your Elasticsearch cluster. CORS can be configured in the [YML File Configuration](#section_8wn_rid_he1) panel. The following table lists the parameters that you can configure.

|Parameter|Default value|Description|
|---------|-------------|-----------|
|http.cors.enabled|false|Specifies whether to enable CORS. CORS allows browsers on other origins to access your Elasticsearch cluster. Valid values:-   true: indicates that CORS is enabled. In this case, the system can process OPTIONS CORS requests. If the origin in a request is declared in http.cors.allow-origin, the system returns a response whose header contains Access-Control-Allow-Origin.
-   false: indicates that CORS is disabled. In this case, the system ignores the origin in the request header and returns a response whose header does not contain Access-Control-Allow-Origin. If a client cannot send a pre-flight request that has origin information included in the request header or does not verify Access-Control-Allow-Origin in the response header returned from the server, the cross-origin security is compromised. If CORS is disabled, a client can send only an OPTIONS request to check whether Access-Control-Allow-Origin exists. |
|http.cors.allow-origin|""|Specifies the origins from which requests are allowed. By default, no origin is allowed. You can set this parameter to a regular expression, such as /https?:\\/\\/localhost\(:\[0-9\]+\)?/. This regular expression indicates that the system responds to requests that match the regular expression. **Note:** Asterisks \(\*\) are valid characters but may cause security risks. If an asterisk is used, your cluster is open to all origins. We recommend that you do not use asterisks. |
|http.cors.max-age|1728000 \(20 days\)|Browsers can send OPTIONS requests to query CORS configurations. This parameter specifies the cache duration of the retrieved CORS configurations. Unit: seconds.|
|http.cors.allow-methods|OPTIONS, HEAD, GET, POST, PUT, DELETE|Specifies the request methods.|
|http.cors.allow-headers|X-Requested-With, Content-Type, Content-Length|Specifies the request headers.|
|http.cors.allow-credentials|false|The credential configuration item. This item specifies whether Access-Control-Allow-Credentials can be contained in the response header. Valid values:-   true: indicates that Access-Control-Allow-Credentials can be contained in the response header.
-   false: indicates that Access-Control-Allow-Credentials cannot be contained in the response header. |

## Configure a remote reindex whitelist

Before you call the reindex API to migrate index data from a remote Elasticsearch cluster to the current Elasticsearch cluster, you must configure a remote reindex whitelist for the current Elasticsearch cluster. You can configure the whitelist in the [YML File Configuration](#section_8wn_rid_he1) panel. The following table describes the parameter that you can configure.

|Parameter|Default value|Description|
|---------|-------------|-----------|
|reindex.remote.whitelist|\[\]|Specifies the remote cluster of the current cluster. Specify a remote cluster in the format of Host address:Port number. If you want to specify multiple remote clusters, separate them with commas \(,\), such as otherhost:9200,another:9200,127.0.10.\*\*:9200,localhost:\*\*. The whitelist does not identify protocols. |

If the remote cluster is a single-zone Alibaba Cloud Elasticsearch cluster, specify this cluster in the <Domain name of the cluster\>:9200 format. If the remote cluster is a multi-zone Alibaba Cloud Elasticsearch cluster, specify this cluster by using the IP addresses of all data nodes in the cluster and the port number of the cluster. The following description provides configuration examples:

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

    **Note:** After the remote reindex whitelist is configured, you can call the reindex API to reindex data. For more information, see [Use the reindex operation to migrate data](/intl.en-US/Best Practices/Elasticsearch migration/Migrate data between Alibaba Cloud Elasticsearch clusters/Use the reindex operation to migrate data.md).


## Configure the Audit Log Indexing feature

You can enable the Audit Log Indexing feature in the [YML File Configuration](#section_8wn_rid_he1) panel to view the related logs. After the feature is enabled, you can customize its configurations. The following code provides sample configurations:

```
xpack.security.audit.index.bulk_size: 5000
xpack.security.audit.index.events.emit_request_body: false
xpack.security.audit.index.events.exclude: run_as_denied,anonymous_access_denied,realm_authentication_failed,access_denied,connection_denied
xpack.security.audit.index.events.include: authentication_failed,access_granted,tampered_request,connection_granted,run_as_granted
xpack.security.audit.index.flush_interval: 180s
xpack.security.audit.index.rollover: hourly
xpack.security.audit.index.settings.index.number_of_replicas: 1
xpack.security.audit.index.settings.index.number_of_shards: 10
```

|Configuration item|Default value|Description|
|------------------|-------------|-----------|
|xpack.security.audit.index.bulk\_size|1000|You can write audit events to audit log indexes in batches. This parameter specifies the maximum number of audit events that can be written in each batch.|
|xpack.security.audit.index.flush\_interval|1s|Specifies the frequency at which buffered audit events are flushed to audit log indexes.|
|xpack.security.audit.index.rollover|daily|Specifies the frequency at which audit events are rolled over to a new audit log index. Valid values: hourly, daily, weekly, and monthly.|
|xpack.security.audit.index.events.include|access\_denied, access\_granted, anonymous\_access\_denied, authentication\_failed, connection\_denied, tampered\_request, run\_as\_denied, run\_as\_granted|Specifies the types of audit events that can be written to audit log indexes. For more information about audit event types, see [Audit Event Types](https://www.elastic.co/guide/en/x-pack/5.5/auditing.html#audit-event-types).|
|xpack.security.audit.index.events.exclude|null, which indicates that the system does not process audit events|Specifies the types of audit events that cannot be written to audit log indexes.|
|xpack.security.audit.index.events.emit\_request\_body|false|Specifies whether to ignore or include REST request bodies when a specific audit event is triggered, such as authentication\_failed.|

**Note:**

-   If an audit event contains the request bodies, sensitive data may be exposed.
-   After the Audit Log Indexing feature is enabled, audit events are stored in the audit log indexes of your cluster. The names of the indexes start with .security\_audit\_log-. These indexes consume the storage of your cluster. Elasticsearch does not automatically clear expired indexes. You must manually clear expired audit log indexes.

You can also use the xpack.security.audit.index.settings parameter to configure the indexes that are used to store audit events. The following code provides a configuration example. In this example, the numbers of primary shards and replica shards for the indexes are set to 1.

```
xpack.security.audit.index.settings:
  index:
    number_of_shards: 1
    number_of_replicas: 1
```

**Note:** If you want to customize the configurations for audit log indexes, add the configurations to the YML file configuration after you set xpack.security.audit.enabled to true to enable Audit Log Indexing. If you do not customize the configurations, the system uses the default settings number\_of\_shards: 5 and number\_of\_replicas: 1 to create the indexes.

For more information, see [Auditing Security Settings](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/auditing-settings.html#auditing-settings).

## Configure queue sizes

You can customize queue sizes to adjust the sizes of the document write queue and document search queue. You can configure the queue sizes in the [YML File Configuration](#section_8wn_rid_he1) panel. The following code provides a configuration example. In this example, the size of the document write queue is set to 500 and that of the document search queue is set to 1200. You can change these values based on your business requirements.

```
thread_pool.bulk.queue_size: 500
thread_pool.write.queue_size: 500
thread_pool.search.queue_size: 1200
```

|Parameter|Default value|Description|
|---------|-------------|-----------|
|thread\_pool.bulk.queue\_size|200|The size of the document write queue. This parameter is available for Elasticsearch V5.X.|
|thread\_pool.write.queue\_size|200|The size of the document write queue. This parameter is available for Elasticsearch V6.X and V7.X.|
|thread\_pool.search.queue\_size|1000|The size of the document search queue.|

