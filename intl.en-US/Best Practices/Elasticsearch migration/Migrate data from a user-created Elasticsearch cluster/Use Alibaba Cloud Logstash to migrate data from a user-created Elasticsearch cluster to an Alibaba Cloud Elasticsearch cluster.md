---
keyword: [migrate data from a user-created Elasticsearch cluster, use Alibaba Cloud Logstash to migrate data]
---

# Use Alibaba Cloud Logstash to migrate data from a user-created Elasticsearch cluster to an Alibaba Cloud Elasticsearch cluster

You can use an Alibaba Cloud Logstash pipeline to migrate data from a user-created Elasticsearch cluster to an Alibaba Cloud Elasticsearch cluster. This topic describes the related configurations.

-   A user-created Elasticsearch cluster is prepared.

    We recommend that you create an Elasticsearch cluster on an Alibaba Cloud Elastic Compute Service \(ECS\) instance. For more information, see [Installing and Running Elasticsearch](https://www.elastic.co/guide/cn/elasticsearch/guide/current/running-elasticsearch.html).

    **Note:**

    -   The ECS instance that hosts the user-created Elasticsearch cluster must be deployed in a Virtual Private Cloud \(VPC\). You cannot use an ECS instance that is connected to a VPC over a ClassicLink.
    -   If the user-created Elasticsearch cluster and Logstash cluster reside in different VPCs, configure NAT gateways to connect the ECS instance and Logstash cluster to the Internet. For more information, see [Configure a NAT gateway for data transmission over the Internet](/intl.en-US/Logstash/Network and security/Configure a NAT gateway for data transmission over the Internet.md).
    -   The IP addresses of the nodes in the Logstash cluster are added to the VPC-type security group of the ECS instance that hosts the user-created Elasticsearch cluster. Additionally, port 9200 is enabled. You can obtain the IP addresses on the Basic Information page of the Logstash cluster.
    -   The scripts provided in this topic only apply to scenarios where an Alibaba Cloud Logstash V6.7 cluster is used to migrate data from a user-created Elasticsearch 5.6.16 cluster to an Alibaba Cloud Elasticsearch V6.7 cluster.
-   An Alibaba Cloud Logstash cluster is created.

    For more information, see [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).

-   An Alibaba Cloud Elasticsearch cluster is created in the same VPC, region, and zone as the Logstash cluster. The Elasticsearch cluster has the same version as the Logstash cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

-   The **Auto Indexing** feature is enabled for the Alibaba Cloud Elasticsearch cluster.

    For more information, see [t1605396.md\#section\_pcn\_1xy\_1l2](/intl.en-US/Elasticsearch Instances Management/Access and configure an Elasticsearch cluster.md).


## Configure a Logstash pipeline

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target Logstash cluster. Then, click its ID in the **Cluster ID/Name** column or click **Manage Clusters** in the **Actions** column.

4.  In the left-side navigation pane, click **Pipelines**.

5.  In the **Pipelines** section, click **Create Pipeline**.

    ![Create a pipeline](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5119333951/p95025.png)

6.  In the **Create Task** wizard, configure the pipeline.

    In this topic, the following configurations are used for the pipeline:

    ```
    input {
        elasticsearch {
        hosts => ["http://<IP address of the ECS instance that hosts the user-created Elasticsearch cluster>:9200"]
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
        document_type => "%{[@metadata][_type]}"
        document_id => "%{[@metadata][_id]}"
      }
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |`hosts`|The endpoint of the user-created Elasticsearch cluster. Set this parameter to `http://<IP address of the ECS instance that hosts the user-created Elasticsearch cluster>:<Port>` in the `input` part. Set this parameter to `http://<ID of the Alibaba Cloud Elasticsearch cluster>.elasticsearch.aliyuncs.com:9200` in the `output` part.|
    |`user`|The username that is used to access the user-created Elasticsearch cluster.|
    |`password`|The password that is used to access the user-created Elasticsearch cluster.|
    |`index`|The names of the indexes whose data you want to migrate.|
    |`docinfo`|Set this parameter to `true` to extract the metadata of documents in the user-created Elasticsearch cluster, such as the index, type, and ID.|

    **Note:** In this topic, the default account elastic is used to access the Alibaba Cloud Elasticsearch cluster. If you want to use a custom account, you must first create a role for the account and grant the required permissions to the role. For more information, see [Create a role](/intl.en-US/Best Practices/Elasticsearch applications/Collect data/Use a user-created Logstash instance to synchronize data to Alibaba Cloud Elasticsearch.md) and [Create a user](/intl.en-US/Best Practices/Elasticsearch applications/Collect data/Use a user-created Logstash instance to synchronize data to Alibaba Cloud Elasticsearch.md).

    Parameters in the `output` part are similar to those in the `input` part. If you set the `index` parameter to `%{[@metadata][_index]}` in the output part, the system matches the `index` parameter in the metadata. This indicates that the generated index on the Alibaba Cloud Elasticsearch cluster is the same as the index on the user-created Elasticsearch cluster.

    For more information, see [Logstash configuration files](/intl.en-US/Logstash/Pipeline task management/Logstash configuration files.md).

7.  Click **Next** to configure pipeline parameters.

    ![Configure pipeline parameters](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5119333951/p67293.png)

    |Parameter|Description|
    |---------|-----------|
    |**Pipeline Workers**|The number of worker threads that concurrently run the filter and output plug-ins of the pipeline. If a backlog of events exists or some CPU resources are not used, we recommend that you increase the number of threads to maximize CPU utilization. The default value of this parameter is the number of CPU cores.|
    |**Pipeline Batch Size**|The maximum number of events that a single worker thread can collect from input plug-ins before it attempts to run filter and output plug-ins. If you set this parameter to a large value, a single worker thread can collect more events but consumes larger memory. If you want to ensure that the worker thread has sufficient memory to collect more events, specify the LS\_HEAP\_SIZE variable to increase the Java virtual machine \(JVM\) heap size. Default value: 125.|
    |**Pipeline Batch Delay**|The wait time for an event. This time occurs before you assign a small batch to a pipeline worker thread and after you create batch tasks for pipeline events. Default value: 50. Unit: milliseconds.|
    |**Queue Type**|The internal queue model for buffering events. Valid values:     -   **MEMORY**: traditional memory-based queue. This is the default value.
    -   **PERSISTED**: disk-based ACKed queue, which is a persistent queue. |
    |**Queue Max Bytes**|The value must be less than the total capacity of your disk. Default value: 1024. Unit: MB.|
    |**Queue Checkpoint Writes**|The maximum number of events that are written before a checkpoint is enforced when persistent queues are enabled. The value 0 indicates no limit. Default value: 1024.|

    **Warning:** After you configure the parameters, you must click **Save and Deploy** to make the pipeline take effect. The **Save and Deploy** operation triggers a restart of the Logstash cluster. Before you can proceed, make sure that the restart does not affect your services.

8.  Click **Save** or **Save and Deploy**.

    -   **Save**: After you click this button, the system stores the pipeline settings and triggers a cluster change. However, the settings do not take effect. After you click Save, the **Pipelines** page appears. In the **Pipelines** section, find the created pipeline and click **Deploy** in the **Actions** column. Then, the system restarts the Logstash cluster to make the settings take effect.
    -   **Save and Deploy**: After you click this button, the system restarts the Logstash cluster to make the settings take effect.

## View migration results

1.  Log on to the Kibana console of the Alibaba Cloud Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the `GET /_cat/indices?v` command to view the index that stores migrated data.

    ![Index that stores migrated data](../images/p95323.png)


