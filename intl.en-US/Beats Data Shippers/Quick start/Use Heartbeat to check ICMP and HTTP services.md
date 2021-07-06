---
keyword: [use Heartbeat to check the ICMP service, use Heartbeat to check the HTTP service]
---

# Use Heartbeat to check ICMP and HTTP services

This topic describes how to use Alibaba Cloud Heartbeat to check the status of Internet Control Message Protocol \(ICMP\) and HTTP services. It can also generate visual charts.

Heartbeat is a lightweight daemon. It can be installed on a remote server to check the availability of your services on a regular basis. Heartbeat is unlike Metricbeat. Heartbeat checks whether your services are reachable but Metricbeat checks whether your services are running.

**Note:** Most Beats shippers need to be installed on edge nodes. However, Heartbeat can be installed on a separate computer or even outside the network where services you want to monitor are deployed.

Heartbeat supports the following types of monitors:

-   ICMPv4 or ICMPv6 monitor: sends ICMP requests to check whether a service is available. This type of monitor connects to a service over ICMP. If you want to use an ICMP monitor, root permissions are required.
-   TCP monitor: sends or receives specific workloads to check whether a service is available. This type of monitor connects to a service over TCP.
-   HTTP monitor: checks whether a service is available based on specific status codes, response headers, or response content. This type of monitor connects to a service over HTTP.

    **Note:** TCP and HTTP monitors support Secure Sockets Layer \(SSL\), Transport Layer Security \(TLS\), and some proxy settings.


## Prerequisites

-   An Alibaba Cloud Elasticsearch cluster is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

-   The **Auto Indexing** feature is enabled for the Elasticsearch cluster.

    For security purposes, Alibaba Cloud Elasticsearch disables the **Auto Indexing** feature by default. However, Beats depends on this feature. If you select **Elasticsearch** for **Output** when you create a shipper, you must enable the **Auto Indexing** feature. For more information, see [t1605396.md\#section\_pcn\_1xy\_1l2](/intl.en-US/Elasticsearch Instances Management/Access and configure an Elasticsearch cluster.md).

-   An Alibaba Cloud Elastic Compute Service \(ECS\) instance is created in the same virtual private cloud \(VPC\) as the Elasticsearch cluster.

    For more information, see [Create an instance by using the wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the wizard.md).

    **Note:** Beats supports only Aliyun Linux, Red Hat Linux, and CentOS.

-   Cloud Assistant and Docker are installed on the ECS instance.

    For more information, see [Install the Cloud Assistant client](/intl.en-US/Deployment & Maintenance/Cloud assistant/Configure the Cloud Assistant client/Install the Cloud Assistant client.md) and [Deploy and use Docker on Alibaba Cloud Linux 2 instances](/intl.en-US/Tutorials/Build an application/Deploy and use Docker/Deploy and use Docker on Alibaba Cloud Linux 2 instances.md).


## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the **Create Shipper** section of the page that appears, click **Heartbeat**.

3.  Install and configure the shipper.

    For more information, see [Collect the logs of an ECS instance](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md) and [Prepare the YML configuration files for a shipper](/intl.en-US/Beats Data Shippers/Prepare the YML configuration files for a shipper.md). The following figure shows the configurations that are used in this topic.

    ![Heartbeat configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9557359951/p88191.png)

    **Note:**

    -   If you select **Enable Kibana Monitoring**, the system enables Heartbeat service monitoring in the Kibana console.
    -   If you select **Enable Kibana Dashboard**, the system generates charts in the Kibana console. You do not need to configure the YML file. Alibaba Cloud Kibana is configured in a VPC. You must enable the VPC access feature for Kibana on the Kibana configuration page. For more information, see [Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure an IP address whitelist for access to the Kibana console over the Internet
         or an internal network.md).
    When you configure the shipper, specify `heartbeat.monitors` in **heartbeat.yml** to configure monitors. The following monitor configurations are used in this topic:

    ```
    heartbeat.monitors:
    - type: icmp
      schedule: '*/5 * * * * * *'
      hosts: ["47.111.xx.xx"]
    - type: http
      # List or urls to query
      urls: ["https://es-cn-xxxxx.kibana.elasticsearch.aliyuncs.com:5601/"]
      # Configure task schedule
      schedule: '@every 10s'
      check.response.status: 200
    ```

    |Parameter|Description|
    |---------|-----------|
    |`type`|The monitor type. In the preceding configurations, `icmp` and `http` monitors are specified.|
    |`schedule`|The task schedule. If you set the value to `'*/5 * * * * * *'`, the system runs the task at 5-second intervals. If you set the value to `'@every 10s'`, the system runs the task at 10-second intervals from the time Heartbeat is started.|
    |`hosts`|The servers that you want to ping.|
    |`urls`|The URLs that you want to ping.|
    |`check.response.status`|The expected HTTP status code that is returned for the HTTP request. If you set the value to `200`, the system determines that the related service is normal if `200` is returned.|

    **Note:** For more information, see [open source Heartbeat documentation](https://www.elastic.co/guide/en/beats/heartbeat/6.8/configuration-heartbeat-options.html).

4.  Select the ECS instance on which you want to install the shipper.

    ![Select the ECS instance on which you want to install a shipper](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0657359951/p82419.png)

    The selected ECS instance must meet the preceding prerequisites.

5.  Enable the shipper and check whether the shipper installation succeeds.

    1.  Click **Enable**.

        Then, the **Enable Shipper** message appears.

    2.  Click **Back to Beats Shippers**. In the **Manage Shippers** section of the **Beats Data Shippers** page, view the installed shipper.

    3.  After the **state** of the shipper changes to **Enabled 1/1**, click **View Instances** in the **Actions** column.

    4.  In the **View Instances** pane, check whether the shipper installation on the ECS instance succeeds. If the value of **Installed Shippers** is **Heartbeat Normal**, the shipper installation succeeds.

        ![View shipper installation status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0657359951/p82426.png)


## View the collected data

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Discover**. On the page that appears, select **heartbeat-\*** from the drop-down list in the upper-left corner and specify a period in the upper-right corner. Then, view the data collected by Heartbeat within the specified period.

    ![Data collected by Heartbeat](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0657359951/p88202.png)

3.  In the left-side navigation pane, click **Dashboard**.

4.  In the **Dashboards** section of the page that appears, click **Heartbeat HTTP monitoring**. In the upper-right corner of the page that appears, select a period. Then, view HTTP status statistics within the specified period.

    ![HTTP status monitoring chart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0657359951/p88201.png)


