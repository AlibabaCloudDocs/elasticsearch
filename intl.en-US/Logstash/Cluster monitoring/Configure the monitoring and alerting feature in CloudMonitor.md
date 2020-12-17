---
keyword: [Logstash monitoring and alerting, Logstash cluster monitoring]
---

# Configure the monitoring and alerting feature in CloudMonitor

Alibaba Cloud Logstash allows you to monitor clusters and customize alert thresholds. If an alert is detected, you are notified of the alert through SMS messages. To ensure the stability of your Logstash cluster, we recommend that you configure monitoring and alerting to monitor items in real time, such as cluster status and disk usage. We recommend that you check the SMS messages in time and take appropriate measures in advance.

Logstash allows you to configure monitoring and alerting for the metrics that are described in the following table.

|Monitoring metric|Description|
|-----------------|-----------|
|ClusterStatus|Required. This metric checks the cluster status. Green indicates that a Logstash cluster is in normal status. Yellow and red indicate that a Logstash cluster is in abnormal status.|
|NodeDiskUtilization\(%\)|Required. Set the threshold to a value that is less than 75%.|
|NodeHeapMemoryUtilization\(%\)|Required. Set the threshold to a value that is less than 85%.|
|NodeCPUUtilization\(%\)|Optional. Set the threshold to a value that is less than 95%.|
|NodeLoad\_1m|Optional. Set the threshold to a value that is 80% of the number of CPU cores per node.|
|ClusterQueryQPS\(Count/Second\)|Optional. Set the threshold based on the actual test result.|
|ClusterIndexQPS\(Count/Second\)|Optional. Set the threshold based on the actual test result.|

## Procedure

1.  Log on to the CloudMonitor console.

    You can open the required page by using one of the following methods:

    -   From the Elasticsearch console
        1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).
        2.  In the top navigation bar, select the region where your cluster resides.
        3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.
        4.  On the **Basic Information** page, click **Cluster Monitoring** in the upper-right corner of the page.
    -   From the CloudMonitor console
        1.  Log on to the [CloudMonitor console](https://cloudmonitor.console.aliyun.com/#/cloud/buckets/).
        2.  In the left-side navigation pane, click **Alarms**. Then, click **Alarm Rules**.
        3.  On the **Threshold Value Alarm** tab, click **Create Alarm Rule**.
        4.  On the page that appears, select **AliyunLogstashService** for Product in the **Related Resource** section.
        5.  Select the region and ID of your Alibaba Cloud Logstash cluster.
2.  Configure related resources.

    ![Configure related resources](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7303986061/p67529.png)

    |Parameter|Description|
    |---------|-----------|
    |**Product**|Select **AliyunLogstashService**.|
    |**Resource Range**|Select **Instance**.|
    |**Region**|Select the region where the Logstash cluster resides.|
    |**Instance**|Select the ID of the Logstash cluster.|

3.  Configure alert rules.

    ![Configure alert rules](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7303986061/p67531.png)

    -   The values for cluster states Green \(normal\), Yellow \(alert\), and Red \(unhealthy\) are **0.0**, **1.0**, and **2.0**. Reference these values and set a suitable threshold for the **ClusterStatus** metric.
    -   The **Mute for** parameter specifies the intervals at which an alert notification is re-sent when a threshold is reached.

        **Note:** For more information about other parameters, see [Create a threshold-triggered alert rule](/intl.en-US/Alarm service/Alarm rules/Create a threshold-triggered alert rule.md).

4.  In the **Notification Method** section, select **Default Contact Group** from the Contact Group section and click the rightwards arrow to add it to the Selected Groups section.

    If you do not have an alert group, click **Quickly create a contact group** to create a group.

    ![Quickly create a contact group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667819951/p39988.png)

    **Note:** In the **HTTP CallBack** field, enter a URL that can be accessed from the Internet. CloudMonitor delivers a POST request to push the alert notification to this URL. Only HTTP is supported.

5.  Click **Confirm**.

    After the configurations are complete, the system starts to monitor your Logstash cluster and displays the monitoring data.


