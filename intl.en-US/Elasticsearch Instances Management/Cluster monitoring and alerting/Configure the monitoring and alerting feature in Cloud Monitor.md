---
keyword: Elasticsearch monitoring and alerting
---

# Configure the monitoring and alerting feature in Cloud Monitor

Alibaba Cloud Elasticsearch allows you to monitor clusters and customize alert thresholds. If an alert is detected, the system notifies you of the alert by sending a text message. To ensure the stability of your Elasticsearch cluster, we recommend that you configure monitoring and alerting. This way, the system can monitor items in real time, such as cluster status and disk usage. You can check text messages in time and take measures in advance.

Elasticsearch allows you to configure monitoring and alerting for the metrics that are described in the following table.

|Metric|Description|
|------|-----------|
|ClusterStatus|Required. This metric checks the cluster status. Green indicates that an Elasticsearch cluster is in a normal state. Yellow or red indicate that an Elasticsearch cluster is in an abnormal state.|
|NodeDiskUtilization\(%\)|Required. Set the threshold to a value that is less than 75%. The upper limit is 80%.|
|NodeHeapMemoryUtilization\(%\)|Required. Set the threshold to a value that is less than 85%. The upper limit is 90%.|
|NodeCPUUtilization\(%\)|Optional. Set the threshold to a value that is less than or equal to 95%.|
|NodeLoad\_1m|Optional. Set the threshold to a value that is 80% of the number of CPU cores per node.|
|ClusterQueryQPS\(Count/Second\)|Optional. Set the threshold based on the actual test result.|
|ClusterIndexQPS\(Count/Second\)|Optional. Set the threshold based on the actual test result.|

**Note:** The monitoring and alerting feature is enabled for your Elasticsearch cluster by default. You can view historical monitoring data on the Cluster Monitoring page of your cluster. Only monitoring information that is generated over the last month is displayed.

1.  Go to the Elasticsearch page of the Cloud Monitor console.

    You can use one of the following methods:

    -   From the Elasticsearch console
        1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).
        2.  In the top navigation bar, select the region where your cluster resides.
        3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.
        4.  On the **Basic Information** page, click **Cluster Monitoring** in the upper-right corner.
    -   From the Cloud Monitor console
        1.  Log on to the [Cloud Monitor console](https://cloudmonitor.console.aliyun.com/#/cloud/buckets/elasticsearch/).
        2.  Select the region where your cluster resides.
        3.  On the **Instances** tab, find your cluster and click its ID in the **Instance ID** column.
2.  In the upper-right corner, click **Alert Rules**.

3.  On the page that appears, click **Create Alert Rule**.

4.  On the **Create Alert Rule** page, specify **alert rules**.

    The following example demonstrates how to specify alert rules for **NodeDiskUtilization\(%\)**, **ClusterStatus**, and **NodeHeapMemoryUtilization\(%\)**.

    ![Related Resource step](../images/p39986.png "Related Resource step")

    ![Set Alert Rules step](../images/p39987.png "Set Alert Rules step")

    -   The values for cluster states **Green**, **Yellow**, and **Red** are **0.0**, **1.0**, and **2.0**. Reference these values and set a suitable threshold for the **ClusterStatus** metric.
    -   The **Mute for** parameter specifies the interval at which an alert notification is re-sent when a threshold is reached.

        **Note:** For more information about other parameters, see [Create a threshold-triggered alert rule](/intl.en-US/Alarm service/Alarm rules/Create a threshold-triggered alert rule.md).

5.  In the **Notification Method** step, select **Default Contact Group** from the Contact Group section and click the rightwards arrow to add it to the Selected Groups section.

    If you do not have an alert group, click **Quickly create a contact group** to create a group.

    ![Quickly create a contact group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667819951/p39988.png)

    **Note:** In the **HTTP CallBack** field, enter a URL that can be accessed from the Internet. Cloud Monitor delivers a POST request to this URL to push the alert notification. Only HTTP is supported.

6.  Click **Confirm**.

    Then, the system starts to monitor your Elasticsearch cluster and displays the monitoring data on the Cluster Monitoring page of the cluster.


