---
keyword: [Metricbeat, use Metricbeat to collect system data, use Metricbeat to collect NGINX service data]
---

# Use Metricbeat to collect system data and NGINX service data

This topic describes how to use Alibaba Cloud Metricbeat to collect system data and NGINX service data and then generate visual charts. The system data includes CPU utilization, memory usage, disk I/O, and network I/O.

-   An Alibaba Cloud Elasticsearch cluster is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

-   The **Auto Indexing** feature is enabled for the Elasticsearch cluster.

    For security purposes, Alibaba Cloud Elasticsearch disables the **Auto Indexing** feature by default. However, Beats depends on this feature. If you select **Elasticsearch** for **Output** when you create a shipper, you must enable the **Auto Indexing** feature. For more information, see [t1605396.md\#section\_pcn\_1xy\_1l2](/intl.en-US/Elasticsearch Instances Management/Access and configure an Elasticsearch cluster.md).

-   An Alibaba Cloud Elastic Compute Service \(ECS\) instance is created in the same Virtual Private Cloud \(VPC\) as the Elasticsearch cluster.

    For more information, see [Create an instance by using the wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the wizard.md).

    **Note:** Beats supports only Aliyun Linux, Red Hat Linux, and CentOS.

-   Cloud Assistant and Docker are installed on the ECS instance.

    For more information, see [Install the Cloud Assistant client](/intl.en-US/Deployment & Maintenance/Cloud assistant/Configure the Cloud Assistant client/Install the Cloud Assistant client.md) and [Deploy and use Docker on Alibaba Cloud Linux 2 instances](/intl.en-US/Tutorials/Build an application/Deploy and use Docker/Deploy and use Docker on Alibaba Cloud Linux 2 instances.md).


## Use Metricbeat to collect system data

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the **Create Shipper** section of the page that appears, click **Metricbeat**.

3.  Install and configure the shipper.

    For more information, see [Collect the logs of an ECS instance](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md) and [Prepare the YML configuration files for a shipper](/intl.en-US/Beats Data Shippers/Prepare the YML configuration files for a shipper.md). The following figure shows the configurations that are used in this topic.

    ![Metricbeat](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6557359951/p86406.png)

    **Note:**

    -   If you select **Enable Kibana Monitoring**, the system enables Metricbeat service monitoring in the Kibana console.
    -   If you select **Enable Kibana Dashboard**, the system generates charts in the Kibana console. You do not need to configure the YML file. Alibaba Cloud Kibana is configured in a VPC. You must enable the Kibana private network access feature on the Kibana configuration page. For more information, see [Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure an IP address whitelist for access to the Kibana console over the Internet
         or an internal network.md).
    -   The system module is enabled by default. You do not need to specify **Shipper YML Configuration**.
4.  Click **Next**.

5.  Select the target ECS instance.

    ![Select the target ECS instance](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0657359951/p82419.png)

    **Note:** If you are installing a shipper for the first time, click **Authorize Now**. Then, follow the instructions to authorize Elasticsearch to access ECS instances.

6.  Enable the shipper and check whether the shipper installation succeeds.

    1.  Click **Enable**.

        Then, the **Enable Shipper** message appears.

    2.  Click **Back to Beats Shippers**. In the **Manage Shippers** section of the **Beats Data Shippers** page, view the installed shipper.

    3.  After the state of the shipper changes to **Enabled 1/1**, click **View Instances** in the **Actions** column.

    4.  In the **View Instances** pane, check whether the shipper installation on the ECS instance succeeds. If the value of **Installed Shippers** is **Heartbeat Normal**, the shipper installation succeeds.

        ![Shipper installation succeeded](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6557359951/p86408.png)

7.  View the collected data.

    1.  Log on to the Kibana console of your Elasticsearch cluster.

        For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

    2.  In the left-side navigation pane, click **Dashboard**.

    3.  In the **Dashboards** section, click **\[Metricbeat System\] Overview**. On the page that appears, click the target Metricbeat system and view the dashboard for the system.

        ![Dashboard](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6557359951/p86416.png)


## Use Metricbeat to collect NGINX service data

Prerequisites: The `stub_status` module is enabled for the NGINX service. The `ngx_http_stub_status_module` module measures the number of requests received and that of requests processed by the NGINX service. Therefore, you must enable `stub_status` in the nginx.conf file.

```
location /status {
           stub_status on;
           access_log off;
        }
```

**Note:** The value of `server_status_path` in the metricbeat.yml file must be the same as that of `status` in the nginx.conf file.

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the **Create Shipper** section of the page that appears, click **Metricbeat**.

3.  Install and configure the shipper.

    For more information, see [Collect the logs of an ECS instance](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md) and [Prepare the YML configuration files for a shipper](/intl.en-US/Beats Data Shippers/Prepare the YML configuration files for a shipper.md). The following figure shows the configurations that are used in this topic.

    ![Configure the shipper for the NGINX service](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6557359951/p86418.png)

    Add the following script in the **metricbeat.yml** file:

    ```
    metricbeat.modules:
    - module: nginx
      metricsets: ["stubstatus"]
      enabled: true
      period: 10s
      # Nginx hosts
      hosts: ["http://121.41.**.**"]
      # Path to server status. Default server-status
      server_status_path: "status"
    ```

    **Note:**

    -   If you select **Enable Kibana Monitoring**, the system enables Metricbeat service monitoring in the Kibana console.
    -   If you select **Enable Kibana Dashboard**, the system generates charts in the Kibana console. You do not need to configure the YML file. Alibaba Cloud Kibana is configured in a VPC. You must enable the Kibana private network access feature on the Kibana configuration page. For more information, see [Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure an IP address whitelist for access to the Kibana console over the Internet
         or an internal network.md).
4.  Click **Next**.

5.  Select the target ECS instance.

    ![Select the target ECS instance](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0657359951/p82419.png)

    **Note:** If you are installing a shipper for the first time, click **Authorize Now**. Then, follow the instructions to authorize Elasticsearch to access ECS instances.

6.  Enable the shipper and check whether the shipper installation succeeds.

    For more information, see [Use Metricbeat to collect system data](#section_3rx_xw8_rbi).

7.  View the collected data.

    1.  In the address bar of your browser, enter `<IP address of the NGINX server>/status` to access the monitoring page of the NGINX server.

        ![Enter the monitoring page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6557359951/p86420.png)

    2.  Log on to the Kibana console of your Elasticsearch cluster.

        For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

    3.  In the left-side navigation pane, click **Dashboard**.

    4.  In the **Dashboards** section, click **\[Metricbeat Nginx\] Overview**. On the page that appears, view the dashboard for the NGINX service.

        ![Dashboard for the NGINX service](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6557359951/p86423.png)


