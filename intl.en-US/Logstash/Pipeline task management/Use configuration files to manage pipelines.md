---
keyword: [Logstash pipeline configuration, use Logstash configuration file to manage pipelines]
---

# Use configuration files to manage pipelines

If you want to use Alibaba Cloud Logstash to collect data, you can use the configuration file of a Logstash cluster to create and configure pipelines. This method allows you to run a maximum of 20 pipelines at the same time. This topic describes how to use the configuration file of a Logstash cluster to manage pipelines.

-   An Alibaba Cloud Elasticsearch cluster is created, and the Auto Indexing feature is enabled for the cluster.

    For more information about how to create an Elasticsearch cluster, see [Create clusters](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create clusters.md). For more information about how to enable the Auto Indexing feature, see [Enable the Auto Indexing feature](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 2: (Optional) Configure a cluster.md).

    **Note:** To ensure data security, Alibaba Cloud Elasticsearch disables the **Auto Indexing** feature by default. When you use Logstash to migrate data to an Elasticsearch cluster, indexes are created by submitting data instead of calling the [create index operation](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html). Therefore, before you use Logstash to migrate data, you must enable the **Auto Indexing** feature for the destination Elasticsearch cluster.

-   An Alibaba Cloud Logstash cluster is created.

    For more information, see [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).


Logstash allows you to configure pipelines by using the following methods:

-   Use the configuration file specified by `-f<path/to/file>` to configure a Logstash pipeline.
-   Use the pipelines.yml configuration file to enable the parallel running of pipelines in the same process.
-   Use Kibana to access Logstash and configure a single-process pipeline.

## Create a pipeline

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Pipelines**.

5.  In the **Pipelines** section, click **Create Pipeline**.

    ![Create a pipeline](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5119333951/p95025.png)

6.  In the **Config Settings**, specify **Pipeline ID** and Config Settings.

    Example:

    ```
    input {
        kafka {
        bootstrap_servers => ["192.168.xx.xx:9092,192.168.xx.xx:9092,192.168.xx.xx:9092"]
        group_id => "group_1"
        topics => ["logstash_test"]
        consumer_threads => 6
        decorate_events => true
        }
    }
    output {
    elasticsearch {
    hosts => ["http://es-cn-o40xxxxxxxxxx****.elasticsearch.aliyuncs.com:9200"]
    index => "logstash_test_1"
    password => "es_password"
    user => "elastic"
    }  
    file_extend {
         path => "/ssd/1/ls-cn-v0h1kzca****/logstash/logs/debug/test"
       }
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |`input`|Specifies the input data source. For more information about supported data source types, see [Input plugins](https://www.elastic.co/guide/en/logstash/7.4/input-plugins.html). **Note:**

    -   The input plug-ins can listen to only the ports from 8000 to 9000 on the node where the Logstash process is running.
    -   If you want to define a plug-in, driver, or file in the input part, perform the following steps: Click **Show Third-party Libraries**. In the **Third-party Libraries** dialog box, click **Upload**. Then, upload files as prompted. For more information, see [Configure third-party libraries](/intl.en-US/Logstash/Cluster configuration/Configure third-party libraries.md). |
    |`filter`|Specifies the plug-in that is used to filter input data. For more information about supported plug-ins, see [Filter plugins](https://www.elastic.co/guide/en/logstash/7.4/filter-plugins.html).|
    |`output`|Specifies the output data source. For more information about supported data source types, see [Output plugins](https://www.elastic.co/guide/en/logstash/7.4/output-plugins.html). The `file_extend` parameter in the output part is used to enable the pipeline configuration debugging feature. You can use the `path` field to specify the path that stores debug logs. For more information, see [t1898579.md\#]().

**Note:** By default, the `path` field is set to a system-specified path. We recommend that you do not change it. You can click **Start Configuration Debug** to obtain the `path`. |

    For more information about pipeline configurations, see [Logstash configuration files](/intl.en-US/Logstash/Pipeline task management/Logstash configuration files.md).

    **Note:**

    -   For security purposes, if you use a JDBC driver to configure a pipeline, you must add `allowLoadLocalInfile=false&autoDeserialize=false` at the end of the `jdbc_connection_string` parameter, such as `jdbc_connection_string => "jdbc:mysql://xxx.drds.aliyuncs.com:3306/test-database?allowLoadLocalInfile=false&autoDeserialize=false"`. Otherwise, the system displays an error message that indicates a check failure.
    -   If a parameter similar to `last_run_metadata_path` exists in the Config Settings field, the file path must be provided by Logstash. The /ssd/1/ls-cn-xxxxxxx/logstash/data/ path at the backend is available for tests, and the system does not delete the data in this path. Make sure that your disk has sufficient storage space when you use this path.
    -   The Logstash cluster is deployed in a virtual private cloud \(VPC\). If other Alibaba Cloud services are involved in pipeline configuration, we recommend that you select the instances in the same VPC. If you want to allow access to your Logstash cluster over the Internet, configure network and security settings. For more information, see [Configure a NAT gateway for data transmission over the Internet](/intl.en-US/Logstash/Network and security/Configure a NAT gateway for data transmission over the Internet.md).
7.  Click **Next** to configure pipeline parameters.

    ![Configure pipeline parameters](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5119333951/p67293.png)

    |Parameter|Description|
    |---------|-----------|
    |**Pipeline Workers**|The number of worker threads that concurrently run the filter and output plug-ins of the pipeline. If a backlog of events exists or some CPU resources are not used, we recommend that you increase the number of threads to maximize CPU utilization. The default value of this parameter is the number of CPU cores.|
    |**Pipeline Batch Size**|The maximum number of events that a single worker thread can collect from input plug-ins before it attempts to run filter and output plug-ins. If you set this parameter to a large value, a single worker thread can collect more events but consumes larger memory. If you want to ensure that the worker thread has sufficient memory to collect more events, specify the LS\_HEAP\_SIZE variable to increase the JVM heap size. Default value: 125.|
    |**Pipeline Batch Delay**|The wait time for an event. This time occurs before you assign a small batch to a pipeline worker thread and after you create batch tasks for pipeline events. Default value: 50. Unit: milliseconds.|
    |**Queue Type**|The internal queue model for buffering events. Valid values:     -   **MEMORY**: traditional memory-based queue. This is the default value.
    -   **PERSISTED**: disk-based ACKed queue, which is a persistent queue. |
    |**Queue Max Bytes**|The value must be less than the total capacity of your disk. Default value: 1024. Unit: MB.|
    |**Queue Checkpoint Writes**|The maximum number of events that are written before a checkpoint is enforced when persistent queues are enabled. The value 0 indicates no limit. Default value: 1024.|

    **Warning:** After you configure the parameters, you must click **Save and Deploy** to make the pipeline take effect. The **Save and Deploy** operation triggers a restart of the Logstash cluster. Before you can proceed, make sure that the restart does not affect your services.

8.  Click **Save** or **Save and Deploy**.

    -   **Save**: After you click this button, the system stores the pipeline settings and triggers a cluster change. However, the settings do not take effect. After you click Save, the **Pipelines** page appears. In the **Pipelines** section, find the created pipeline and click **Deploy** in the **Actions** column. Then, the system restarts the Logstash cluster to make the settings take effect.
    -   **Save and Deploy**: After you click this button, the system restarts the Logstash cluster to make the settings take effect.
9.  In the message that appears, click **OK**. Then, you can view the created pipeline in the **Pipelines** section.

    After the cluster is restarted, the pipeline creation task is complete.


## Modify a pipeline

**Warning:** If you click **Save and Deploy** after you modify a pipeline, the system restarts the Logstash cluster. Before you can proceed, make sure that the restart does not affect your services.

1.  In the **Pipelines** section, find the pipeline that you want to modify and click **Modify** in the **Actions** column.

2.  In the **Modify** wizard, modify the settings in the **Config Settings** and **Pipeline Parameters** steps. You cannot change the value of **Pipeline ID**.

3.  Click **Save** or **Save and Deploy**. After the cluster is restarted, the pipeline modification task is complete.


## Copy a pipeline

**Warning:** If you click **Save and Deploy** after you copy a pipeline, the system restarts the Logstash cluster. Before you can proceed, make sure that the restart does not affect your services.

1.  In the **Pipelines** section, find the pipeline that you want to copy, click More in the **Actions** column, and then select **Copy**.

2.  In the **Copy** wizard, specify **Pipeline ID** and retain other settings.

3.  Click **Save** or **Save and Deploy**. After the cluster is restarted, the pipeline copy task is complete.


## Delete a pipeline

**Warning:**

-   After a pipeline is deleted, it cannot be recovered, and related ongoing pipeline tasks are stopped. Before you can proceed, make sure that the deletion does not affect your services.
-   The deletion triggers an update of the Logstash cluster. Before you can proceed, make sure that the update does not affect your services.

1.  In the **Pipelines** section, find the pipeline that you want to delete and choose **More** \> **Delete** in the **Actions** column.

2.  In the **Delete Pipeline** wizard, check risk warnings.

3.  Click **OK**. After the cluster is updated, the pipeline deletion task is complete.


