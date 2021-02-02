---
keyword: [Logstash data synchronization, configuration file management of Logstash, pipeline configuration]
---

# Step 2: Create and run a data synchronization task

After a Logstash cluster is created, you can create and run a Logstash pipeline to synchronize data. This topic describes the procedure in detail.

-   Preparations are complete.

    For more information, see [Preparations](/intl.en-US/Logstash/Quick Start/Preparations.md).

-   An Alibaba Cloud Logstash cluster is created.

    For more information, see [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).


## Procedure

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Logstash Clusters**.

3.  In the top navigation bar, select a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the left-side navigation pane, click **Pipelines**.

5.  In the **Pipelines** section, click **Create Pipeline**.

    ![Create a pipeline](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5119333951/p95025.png)

6.  In the **Config Settings** step, specify **Pipeline ID** and Config Settings.

    The following configurations are used in this topic:

    ```
    input {
        elasticsearch {
        hosts => ["http://es-cn-0pp1f1y5g000h****.elasticsearch.aliyuncs.com:9200"]
        user => "elastic"
        password => "your_password"
        index => "*"
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
        document_type => "%{[@metadata][_type]}"
        document_id => "%{[@metadata][_id]}"
      }
      file_extend {
         path => "/ssd/1/ls-cn-v0h1kzca****/logstash/logs/debug/test"
       }
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |hosts|The endpoint of the Alibaba Cloud Elasticsearch cluster. In the input part, the value of this parameter is http://<ID of the source Alibaba Cloud Elasticsearch cluster\>.elasticsearch.aliyuncs.com:9200. In the output part, the value is http://<ID of the destination Alibaba Cloud Elasticsearch cluster\>.elasticsearch.aliyuncs.com:9200.|
    |user|The username that is used to access the Elasticsearch cluster. The default username is elastic.|
    |password|The password that is used to access the Elasticsearch cluster. The password that you specified for the elastic account when you create the cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting a password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|
    |index|The name of the source or destination index whose data you want to migrate. The value %\{\[@metadata\]\[\_index\]\} indicates that the system matches the index parameter in the metadata. In this case, the name of the destination index is the same as that of the source index.|
    |docinfo|Set this parameter to true to extract the metadata of Elasticsearch documents, such as the index, type, and ID.|
    |document\_type|The type of the destination index. The value %\{\[@metadata\]\[\_type\]\} indicates that the system matches the document\_type parameter in the metadata. In this case, the type of the destination index is the same as that of the source index.|
    |document\_id|The IDs of documents in the destination index. The value %\{\[@metadata\]\[\_id\]\} indicates that the system matches the document\_id parameter in the metadata. In this case, the IDs of the documents in the destination index are the same as those in the source index.|
    |file\_extend|Enables the pipeline configuration debugging feature. You can use the path field to specify the path that stores debug logs. Before you use this feature, you must install the logstash-output-file\_extend plug-in. For more information, see [Use the pipeline configuration debugging feature](/intl.en-US/Logstash/Pipeline task management/Use the pipeline configuration debugging feature.md). **Note:** By default, the path field is set to a system-specified path. We recommend that you do not change it. You can click **Start Configuration Debug** to obtain the path. |

    For more information about configurations in the Config Settings step, see [Logstash configuration files](/intl.en-US/Logstash/Pipeline task management/Logstash configuration files.md).

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
9.  In the message that appears, click **OK**.

    Then, you can view the created pipeline on the **Pipelines** page. After the state of the pipeline changes to **Running**, your Logstash cluster starts the data synchronization task.

    ![Pipelines page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6936186061/p85390.png)


View the data synchronization results. For more information, see [Step 3: View data synchronization results](/intl.en-US/Logstash/Quick Start/Step 3: View data synchronization results.md).

