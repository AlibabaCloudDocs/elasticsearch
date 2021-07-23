---
keyword: [migrate data in a self-managed Elasticsearch cluster, use Alibaba Cloud Logstash to migrate data]
---

# Use Alibaba Cloud Logstash to migrate data from a self-managed Elasticsearch cluster to an Alibaba Cloud Elasticsearch cluster

You can use an Alibaba Cloud Logstash pipeline to migrate data from a self-managed Elasticsearch cluster to an Alibaba Cloud Elasticsearch cluster. This topic describes the migration procedure in detail.

-   A self-managed Elasticsearch cluster is created.

    We recommend that you create a self-managed Elasticsearch cluster on Alibaba Cloud Elastic Compute Service \(ECS\) instances. For more information, see [Install and Run Elasticsearch](https://www.elastic.co/guide/cn/elasticsearch/guide/current/running-elasticsearch.html).

    **Note:**

    -   The ECS instances that host the self-managed Elasticsearch cluster must be deployed in a virtual private cloud \(VPC\). You cannot use ECS instances that are connected to a VPC over ClassicLink.
    -   Before you configure a Logstash pipeline, you must check whether the self-managed Elasticsearch cluster resides in the same VPC as the Alibaba Cloud Logstash cluster that you want to use. If they are in different VPCs, you must configure NAT gateways to connect the ECS instances and Logstash cluster to the Internet. For more information, see [Configure a NAT gateway for data transmission over the Internet](/intl.en-US/Logstash/Network and security/Configure a NAT gateway for data transmission over the Internet.md).
    -   You must configure security group rules to allow access from the IP addresses of the nodes in the Logstash cluster for the security groups of the ECS instances that host the self-managed Elasticsearch cluster. In addition, you must enable port 9200. You can obtain the IP addresses of the nodes in the Logstash cluster on the Basic Information page of the Logstash cluster.
    -   The scripts provided in this topic apply only to scenarios in which an Alibaba Cloud Logstash V6.7.0 cluster is used to migrate data from a self-managed Elasticsearch 5.6.16 cluster to an Alibaba Cloud Elasticsearch V6.7.0 cluster.
-   An Alibaba Cloud Logstash cluster is created.

    For more information, see [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).

-   An Alibaba Cloud Elasticsearch cluster is created. Make sure that the Alibaba Cloud Elasticsearch cluster is of the same version and resides in the same VPC as the Logstash cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

-   The Auto Indexing feature is enabled for the Alibaba Cloud Elasticsearch cluster.

    For more information, see [Configure the YML file](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md).


## Configure and run a Logstash pipeline

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the **Logstash Clusters** page, find the desired cluster and click its ID.

4.  In the left-side navigation pane, click **Pipelines**.

5.  In the **Pipelines** section, click **Create Pipeline**.

    ![Create a pipeline](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5119333951/p95025.png)

6.  In the **Create Task** wizard, enter a pipeline ID and configure the pipeline.

    In this example, the following configurations are used for the pipeline:

    ```
    input {
        elasticsearch {
        hosts => ["http://<IP address of the master node in the self-managed Elasticsearch cluster>:9200"]
        user => "elastic"
        index => "*,-.monitoring*,-.security*,-.kibana*"
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
    |hosts|The endpoint of the self-managed Elasticsearch cluster or Alibaba Cloud Elasticsearch cluster. In the input part, specify this parameter in the format of `http://<IP address of the master node in the self-managed Elasticsearch cluster>:<Port number>`. In the output part, specify this parameter in the format of `http://<ID of the Alibaba Cloud Elasticsearch cluster>.elasticsearch.aliyuncs.com:9200`.|
    |user|The username that is used to access the self-managed Elasticsearch cluster or Alibaba Cloud Elasticsearch cluster. **Note:**

    -   The user and password parameters are required in most cases. If the X-Pack plug-in is not installed on the self-managed Elasticsearch cluster, you can leave the two parameters empty.
    -   The default username that is used to access the Alibaba Cloud Elasticsearch clusters is elastic. The default username is used in this example. You can use a custom username. Before you use a custom username, you must create a role for it and grant the required permissions to the role. For more information, see [Create a role](/intl.en-US/RAM/Manage Kibana role/Create a role.md) and [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md). |
    |password|The password that is used to access the self-managed Elasticsearch cluster or Alibaba Cloud Elasticsearch cluster.|
    |index|The names of the indexes whose data you want to migrate or to which you want to migrate data. If you set this parameter to \*,-.monitoring\*,-.security\*,-.kibana\* in the input part, the system migrates data in indexes other than system indexes whose names start with a period \(`.`\). If you set this parameter to %\{\[@metadata\]\[\_index\]\} in the output part, the system matches the index parameter in the metadata. This indicates that the names of the indexes generated on the Alibaba Cloud Elasticsearch cluster are the same as the names of the indexes on the self-managed Elasticsearch cluster.|
    |docinfo|If you set this parameter to true, the system extracts the metadata of documents in the self-managed Elasticsearch cluster, such as the index, type, and id fields.|
    |document\_type|If you set this parameter to %\{\[@metadata\]\[\_type\]\}, the system matches the index type in the metadata. This indicates that the type of the indexes generated on the Alibaba Cloud Elasticsearch cluster is the same as the type of the indexes on the self-managed Elasticsearch cluster.|
    |document\_id|If you set this parameter to %\{\[@metadata\]\[\_id\]\}, the system matches the document IDs in the metadata. This indicates that the IDs of the documents generated on the Alibaba Cloud Elasticsearch cluster are the same as the IDs of the documents on the self-managed Elasticsearch cluster.|

    For more information about how to configure parameters in the Config Settings field, see [Logstash configuration files](/intl.en-US/Logstash/Pipeline task management/Logstash configuration files.md).

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

3.  On the **Console** tab of the page that appears, run the `GET /_cat/indices?v` command to view the indexes that store the migrated data.

    ![Index that stores the migrated data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9485265261/p95323.png)


## FAQ

Q: How do I connect the ECS instances that host the self-managed Elasticsearch cluster to the Alibaba Cloud Logstash cluster when the ECS instances and the Logstash cluster belong to different accounts?

A: The ECS instances and the Logstash cluster belong to different accounts. Therefore, the ECS instances and the Logstash cluster reside in different VPCs. In this case, you can use Cloud Enterprise Network \(CEN\) to connect the ECS instances to the Logstash cluster. For more information, see [Step 3: Attach network instances]().

