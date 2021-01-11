# Use Kibana to manage pipelines for Logstash clusters of earlier versions

From the current version on, new clusters of Logstash do not support Kibana for pipeline management. If your cluster is of an earlier version, we recommended that you use configuration files to manage pipelines. If you use Kibana for pipeline management, you can associate your Logstash cluster with an Elasticsearch cluster and then log on to the Kibana console of the Elasticsearch cluster to manage pipelines.

You have completed the following operations:

-   An Elasticsearch cluster is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

-   A Logstash cluster is created.

    For more information, see [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).


## Associate with an Elasticsearch cluster

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Logstash Clusters**.

3.  In the top navigation bar, select a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the left-side navigation pane, click **Pipelines**.

5.  In the **Pipeline Management** section, click **Modify** next to **Configuration File**.

    In earlier versions, Logstash supports two methods for managing pipelines: by using Kibana and by using configuration files. The latter is the default method. For security purposes, the Kibana-based management method is taken offline for new versions of Logstash clusters. We recommend that you use configuration files to manage pipelines. For more information, see [Use configuration files to manage pipelines](/intl.en-US/Logstash/Pipeline task management/Use configuration files to manage pipelines.md).

6.  On the **Edit Configuration** page, select **Kibana** for **Pipeline Management Method**.

    **Warning:** Changing the method of pipeline management invalidates all pipelines that are configured in the Logstash cluster, and ongoing data tasks are also affected. Before you change the method, you must delete all pipelines managed by this method.

7.  Select the Elasticsearch cluster and enter the username and password of the cluster.

    Enter the username for accessing the Elasticsearch cluster and the password that is specified when the cluster is created. In most cases, the username is elastic.

8.  Click **Test Connectivity and Query Pipelines**.

    After Elasticsearch cluster is connected, the system displays **Pipeline ID**.

9.  Select a value from the **Pipeline ID** drop-down list.

    If you do not have a pipeline ID, click **Log on to Kibana console** to create a pipeline. You are then redirected to the Kibana console of the Elasticsearch cluster. For more information, see [Manage pipelines in the Kibana console](#section_2h6_sur_165).

    After the pipeline is created, return to the **Edit Configuration** page of the Logstash cluster and click **Test Connectivity and Query Pipelines** again to obtain the ID of the created pipeline.

    **Warning:** Changing the configuration of a pipeline triggers a restart of the Logstash cluster. Before you can proceed, make sure that the restart does not affect your services.

10. Select the check box for precautions in the Logstash restart and click **OK**.

    The Logstash cluster is then restarted. During the restart, you can view the restart progress in the [Tasks](/intl.en-US/Logstash/Cluster management/Restart a cluster or node.md) dialog box. After the cluster is restarted, it is associated with the Elasticsearch cluster and data transmission starts.


## Manage pipelines in the Kibana console

After the Elasticsearch cluster is associated, you can create or modify a pipeline in the Kibana console of the associated Elasticsearch cluster. For more information, see [Associate with an Elasticsearch cluster](#section_og5_ink_3wr).

1.  On the **Pipelines** page, click **Kibana Console** next to **Connect to Elasticsearch Cluster**.

2.  On the page that appears, enter the username and password, and click Log in.

3.  In the left-side navigation pane of the Kibana console, click **Management**.

4.  In the **Logstash** section, click **Pipelines**.

5.  In the **Pipelines** section, click **Create pipeline**.

6.  On the **Create Pipeline** page, enter values in **Pipeline ID** and **Description**, and set other parameters.

    You can move the pointer over a parameter to view the parameter description.

    |Parameter|Description|
    |---------|-----------|
    |**Pipeline ID**|The name of the pipeline.|
    |**Description**|The description of the pipeline.|
    |**Pipeline**|The configuration of the pipeline. You must specify input and output source addresses. Example:     ```
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
hosts => ["http://es-cn-o40xxxxxxxxxxxxwm.elasticsearch.aliyuncs.com:9200"]
index => "logstash_test_1"
password => "es_password"
user => "elastic"
}
}
    ``` |
    |**Pipeline workers**|The number of parallel worker threads that you use to execute the filters and output plug-ins of the pipeline.|
    |**Pipeline batch size**|The maximum number of events that a single worker collects before the filter and output plug-ins are executed.|
    |**Pipeline batch delay**|The waiting time for an event before a small batch is assigned to a worker thread. Unit: milliseconds.|
    |**Queue type**|The internal queue model for buffering events. You can select either memory or persisted. Value memory indicates a memory-based queue. Value persisted indicates a disk-based ACKed queue.|
    |**Queue max bytes**|The total capacity of the queue. Default value: 1024 MB. This value equals 1 GB. You can select a unit for the capacity from the drop-down list next to this parameter.|
    |**Queue checkpoint writes**|The maximum number of events that are written before a checkpoint is enforced when persistent queues are enabled.|

7.  Click **Create and deploy**.

    After the pipeline is created, the system returns to the **Pipelines** page. You can view the created pipeline on this page.

8.  Click the ID of the created pipeline. You can modify the pipeline settings on the pipeline details page.


