---
keyword: [Logstash pipeline configuration, Logstash pipeline management by using configuration files]
---

# Use configuration files to manage pipelines

This topic describes how to use the configuration files of Alibaba Cloud Logstash to manage pipelines. This method allows you to run up to 20 pipelines in parallel.

You have completed the following operations:

-   An Elasticsearch cluster is created.

    For more information, see [Create an Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

-   The Auto Indexing feature is enabled for the Elasticsearch cluster.

    For more information, see [Enable the Auto Indexing feature](/intl.en-US/Quick Start/Step 2: (Optional) Configure a cluster.md).

    **Note:** To ensure data security, Elasticsearch sets **Auto Indexing** to **Disable** by default. Instead of calling the Create index operation, Logstash submits data to Elasticsearch, which then automatically indexes the data. Before you use Logstash to transfer data to Elasticsearch, you must set **Auto Indexing** to **Enable** for your Elasticsearch cluster.

-   A Logstash cluster is created.

    For more information, see [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).


Logstash allows you to configure pipelines by using the following methods:

-   Use the configuration file specified by `-f<path/to/file>` to configure a Logstash pipeline.
-   Use the pipelines.yml configuration file to enable the parallel execution of pipelines in the same process.
-   Use Kibana to access Logstash and configure a single-process pipeline.

## Create a pipeline

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Pipelines**.

5.  In the **Pipelines** section, click **Create Pipeline**.

    ![Create Pipeline](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5119333951/p95025.png)

6.  In the **Config Settings** step, configure the pipeline.

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
    hosts => ["es-cn-o40xxxxxxxxxxxxwm.elasticsearch.aliyuncs.com:9200"]
    index => "logstash_test_1"
    password => "es_password"
    user => "elastic"
    }
    }
    ```

    For more information about pipeline configurations, see [Logstash configuration files]().

    **Note:**

    -   The input plug-ins can listen to only the ports from 8000 to 9000 on the node where the Logstash process is running.
    -   If you want to define a plug-in, driver, or file in the input part, follow these steps: Click **Show Third-party Libraries**. In the **Third-party Libraries** dialog box, click **Upload**. Upload the target file as prompted. For more information, see [Configure third-party files](/intl.en-US/Logstash/Cluster configuration/Configure third-party files.md).
    -   If you have used the JDBC driver to configure a pipeline, you must append `allowLoadLocalInfile=false&autoDeserialize=false` following `jdbc_connection_string` to improve security. Otherwise, when you add a Logstash configuration file, the scheduling system reports a verification failure error. Example: `jdbc_connection_string => "jdbc:mysql://xxx.drds.aliyuncs.com:3306/test-database? allowLoadLocalInfile=false&autoDeserialize=false"`.
    -   If the Config Settings step contains a parameter similar to `last_run_metadata_path`, the file path must be provided by Logstash. The Logstash backend provides /ssd/1/ls-cn-xxxxxxx/logstash/data/ for test purposes. Data stored in this path will not be deleted. Make sure that your disk has sufficient storage space when you use this path.
    -   The Logstash cluster is created in a Virtual Private Cloud \(VPC\). If other Alibaba Cloud services are involved in pipeline configuration, we recommend that you select the instances in the same VPC. If you want to allow access to Logstash over the Internet, configure the network and security. For more information, see [Configure a NAT gateway for data transmission over the Internet](/intl.en-US/Logstash/Network and security/Configure a NAT gateway for data transmission over the Internet.md).
7.  Click **Next** to set pipeline parameters.

    ![Pipeline Parameters](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5119333951/p67293.png)

    |Parameter|Description|
    |---------|-----------|
    |**Pipeline Workers**|The number of worker threads that execute the filter and output plug-ins of the pipeline in parallel. If a backlog of events exists or CPU resources are sufficient, we recommend that you increase the number of threads to maximize CPU utilization. The default value is the number of CPU cores.|
    |**Pipeline Batch Size**|The maximum number of events that a single worker thread can collect from input plug-ins before it attempts to execute filter and output plug-ins. If the value of this parameter is large, a single worker thread can collect more events but consumes higher memory. If you want to ensure that the worker thread has sufficient memory to collect more events, specify the LS\_HEAP\_SIZE variable to increase the JVM heap size. Default value: 125.|
    |**Pipeline Batch Delay**|The wait time for an event. This time occurs before you assign a small batch to a pipeline worker thread and after you create batch tasks for pipeline events. Unit: milliseconds. Default value: 50.|
    |**Queue Type**|The internal queue model for buffering events. Valid values:     -   **MEMORY**: a traditional memory-based queue. This is the default value.
    -   **PERSISTED**: a disk-based ACKed queue, which is a persistent queue. |
    |**Queue Max Bytes**|The value must be smaller than the total capacity of your disk. Default value: 1024. Unit: MB.|
    |**Queue Checkpoint Writes**|The maximum number of events that are written before a checkpoint is enforced when persistent queues are enabled. The value 0 indicates no limit. Default value: 1024.|

    **Warning:** After you set the parameters, you must save the settings and deploy the pipeline. The Save and Deploy operation triggers a restart of the Logstash cluster. Before you can proceed, make sure that the restart does not affect your services.

8.  Click **Save** or **Save and Deploy**.

    -   **Save**: The pipeline settings are stored in Logstash and a cluster change is triggered. However, pipeline deployment is not triggered. After you click Save, the system returns to the **Pipelines** page. In the **Pipelines** section, click **Deploy** in the **Actions** column to trigger the pipeline deployment.
    -   **Save and Deploy**: If you click this button, the system triggers the pipeline deployment and cluster restart.
9.  In the message that appears, click **OK**. Then, you can view the created pipeline in the **Pipelines** section.

    After the cluster is restarted, the pipeline creation task is complete.


## Modify a pipeline

**Warning:** When you modify a pipeline, saving the changes triggers a restart of the Logstash cluster. Before you can proceed, make sure that the restart does not affect your services.

1.  In the **Pipelines** section, find the target pipeline and click **Modify** in the **Actions** column.

2.  In the **Modify** wizard, modify the settings in the **Config Settings** and **Pipeline Parameters** steps. You cannot change the value of **Pipeline ID**.

3.  Click **Save** or **Save and Deploy**. After the cluster is restarted, the pipeline modification task is complete.


## Copy a pipeline

**Warning:** When you copy a pipeline, saving the settings triggers a restart of the Logstash cluster. Before you can proceed, make sure that the restart does not affect your services.

1.  In the **Pipelines** section, find the target pipeline and click **Copy** in the **Actions** column.

2.  In the **Copy** wizard, specify **Pipeline ID** and retain other settings.

3.  Click **Save** or **Save and Deploy**. After the cluster is restarted, the pipeline copy task is complete.


## Delete a pipeline

**Warning:**

-   After a pipeline is deleted, it cannot be restored, and related ongoing pipeline tasks are stopped. Before you can proceed, make sure that the delete operation does not affect your services.
-   Deleting a pipeline triggers a change of the Logstash cluster. Before you can proceed, make sure that the change does not affect your services.

1.  In the **Pipelines** section, find the target pipeline and choose **More** \> **Delete** in the **Actions** column.

2.  In the **Delete Pipeline** wizard, check risk warnings.

3.  Click **OK**. After the cluster is changed, the pipeline deletion task is complete.


