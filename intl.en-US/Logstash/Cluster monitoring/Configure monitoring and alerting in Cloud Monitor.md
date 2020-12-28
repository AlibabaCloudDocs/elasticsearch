---
keyword: [Logstash monitoring and alerting, monitor Logstash clusters]
---

# Configure monitoring and alerting in Cloud Monitor

Alibaba Cloud Logstash can monitor clusters and allows you to customize alert thresholds. If an alert is detected, the system notifies you of the alert by sending a text message. To ensure the stability of your Logstash cluster, we recommend that you configure monitoring and alerting. This way, the system can monitor items in real time, such as cluster status and disk usage. We recommend that you check text messages in time and take appropriate measures in advance. This topic describes how to configure monitoring and alerting for a Logstash cluster in Cloud Monitor.

Logstash allows you to configure monitoring and alerting for the metrics that are described in the following table.

|Metric|Description|
|------|-----------|
|node disk usage\(%\)|Required. Set the threshold to a value that is less than 75%.|
|node heap memory usage\(%\)|Required. Set the threshold to a value that is less than 85%.|
|node cpu usage\(%\)|Optional. Set the threshold to a value that is less than 95%.|
|node 1m load\(value\)|Optional. Set the threshold to a value that is 80% of the number of CPU cores per node.|

**Note:** Only the preceding metrics can be configured in Cloud Monitor. If you observe other metrics, ignore them.

## Procedure

1.  Go to the Create Alert Rule page.

    1.  Log on to the [Cloud Monitor console](https://cloudmonitor.console.aliyun.com/#/cloud/buckets/).

    2.  In the left-side navigation pane, choose **Alerts** \> **Alert Rules**.

    3.  On the **Threshold Value Alert** tab, click **Create Alert Rule**.

2.  Configure related resources.

    ![Configure related resources](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7303986061/p67529.png)

    |Parameter|Description|
    |---------|-----------|
    |**Product**|Select **AliyunLogstashService**.|
    |**Resource Range**|Select **Instance**.|
    |**Region**|Select the region where your Logstash cluster resides.|
    |**Instance**|Select your Logstash cluster.|

3.  Configure alert rules.

    ![Configure alert rules](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7032219061/p67531.png)

    The **Mute for** parameter specifies the interval at which an alert notification is re-sent when a threshold is reached.

    **Note:** For more information about other parameters, see [Create a threshold-triggered alert rule](/intl.en-US/Alarm service/Alarm rules/Create a threshold-triggered alert rule.md).

4.  In the **Notification Method** step, select **Default Contact Group** from the Contact Group section and click the rightwards arrow to add it to the Selected Groups section.

    If you do not have an alert group, click **Quickly create a contact group** to create a group.

    ![Quickly create an alert group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667819951/p39988.png)

    **Note:** You can enter a URL that can be accessed from the Internet in the **HTTP CallBack** field. Cloud Monitor delivers a POST request to this URL to push the alert notification. Only HTTP is supported.

5.  Click **Confirm**.

    After the configuration is completed and your Logstash cluster is in a normal state, the system starts to monitor the cluster and displays the monitoring data. If the value of a metric exceeds the related threshold, the system sends you an alert notification. You can perform the following steps to view Logstash dashboards:

    1.  In the left-side navigation pane of the Cloud Monitor console, choose **Dashboard** \> **Cloud product charts**.

    2.  In the upper-right corner of the page that appears, select **AliyunLogstashService** from the drop-down list and select the region where your Logstash cluster resides.

    3.  Select your Logstash cluster from the **Instance** drop-down list and specify a time range to view the monitoring data in this time range.

        ![Logstash dashboards](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7032219061/p170234.png)


