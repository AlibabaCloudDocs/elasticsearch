---
keyword: [data synchronization, configuration file management, pipeline configuration]
---

# Step 2: Create and run a data synchronization task

This topic describes how to use the configuration file management feature of Alibaba Cloud Logstash to create a pipeline and then configure the pipeline to synchronize data.

-   The preparations for using Logstash are complete.

    For more information, see [Preparations](/intl.en-US/Logstash/Quick Start/Preparations.md).

-   An Alibaba Cloud Logstash cluster is created.

    For more information, see [Create an Alibaba Cloud Logstash cluster]().


## Use a configuration file to create a pipeline

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Pipelines**.

5.  In the **Pipelines** section, click **Create Pipeline**.

    ![Create Pipeline](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5119333951/p95025.png)

6.  In the **Config Settings** step of the Create Task wizard, complete configurations.

    The following configurations are used in this topic.

    ```
    input {
        elasticsearch {
        hosts => ["http://es-cn-0pp1f1y5g000h****.elasticsearch.aliyuncs.com:9200"]
        user => "elastic"
        index => "*"
        password => "your_password"
        docinfo => true
      }
    }
    filter {
    }
    output {
      elasticsearch {
        hosts => ["http://es-cn-mp91cbxsm000c****.elasticsearch.aliyuncs.com:9200"]
        user => "elastic"
        password => "your_password"
        index => "%{[@metadata][_index]}"
      }
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |`hosts`|The endpoint of your Alibaba Cloud Elasticsearch cluster. In the `input` part, the value of this parameter is `http://<ID of the source Alibaba Cloud Elasticsearch cluster>.elasticsearch.aliyuncs.com:9200`. In the `output` part, the value is `http://<ID of the destination Alibaba Cloud Elasticsearch cluster>.elasticsearch.aliyuncs.com:9200`.|
    |`user`|The username that is used to access your Alibaba Cloud Elasticsearch cluster.|
    |`password`|The password that is used to access your Alibaba Cloud Elasticsearch cluster.|
    |`index`|The name of the index whose data you want to synchronize.|
    |`docinfo`|Set this parameter to `true` to extract the metadata of Elasticsearch documents, such as the index, type, and ID.|

    Parameters in the `output` part are similar to those in the `input` part. If you set the `index` parameter to `%{[@metadata][_index]}`, the system matches the `index` parameter in the metadata. This indicates that the generated index is the same as the index of the source Elasticsearch cluster. For more information, see [Logstash configuration files]().

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
9.  In the message that appears, click **OK**.

    You can then view the created pipeline in the **Pipelines** section. After the state of the pipeline changes to **Running**, your Logstash cluster starts the data synchronization task.

    ![Pipelines section](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6936186061/p85390.png)


View data synchronization results. For more information, see [Step 3: View data synchronization results]().

