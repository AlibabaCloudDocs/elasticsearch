---
keyword: install a shipper
---

# Collect the logs of an ECS instance

You can use Beats to collect the data of an Elastic Compute Service \(ECS\) instance. The data includes logs, network data, and server metrics. Beats then sends the data to Alibaba Cloud Elasticsearch or Logstash for further processing, such as monitoring and analytics. This topic describes how to use Filebeat to collect the logs of an ECS instance.

-   An Alibaba Cloud Elasticsearch or Logstash cluster is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md) and [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).

-   The Auto Indexing feature is enabled for the Elasticsearch cluster.

    For security purposes, Alibaba Cloud Elasticsearch disables the **Auto Indexing** feature by default. However, Beats depends on this feature when it collects the logs of ECS instances. If you select **Elasticsearch** for **Output**, you must enable the **Auto Indexing** feature for the Elasticsearch cluster. For more information, see [t1605396.md\#section\_pcn\_1xy\_1l2](/intl.en-US/Elasticsearch Instances Management/Access and configure an Elasticsearch cluster.md).

-   An ECS instance is created in the same virtual private cloud \(VPC\) as the Elasticsearch or Logstash cluster.

    When you create the ECS instance, select one of the following operating systems: Alibaba Cloud Linux, Red Hat Enterprise Linux \(RHEL\), and Community Enterprise Operating System \(CentOS\). Beats supports only these three operating systems. For more information, see [Create an instance by using the wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the wizard.md).

    **Note:** The default installation directory of Beats is /opt/aliyunbeats/. After you install Beats, the conf, logs, and data directories are generated on the ECS instance. The conf directory contains the configuration file. The logs directory contains the Beats log file. The data directory contains the Beats data file. We recommend that you do not modify these files. Otherwise, errors may occur, or data may be altered. If an error occurs, you can view the Beats logs in the logs directory to locate the error.

-   Cloud Assistant and Docker are installed on the ECS instance.

    For more information, see [Install the Cloud Assistant client](/intl.en-US/Deployment & Maintenance/Cloud assistant/Configure the Cloud Assistant client/Install the Cloud Assistant client.md) and [Deploy and use Docker on Alibaba Cloud Linux 2 instances](/intl.en-US/Tutorials/Build an application/Deploy and use Docker/Deploy and use Docker on Alibaba Cloud Linux 2 instances.md).


## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select a region. In the left-side navigation pane, click **Beats Data Shippers**.

3.  If this is the first time you go to the **Beats Data Shippers** page, click **Confirm** in the Confirm Service Authorization message to authorize the system to create a service-linked role for your account.

    ![Confirm Service Authorization](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8144790261/p268158.png)

    **Note:** When Beats collects data from various data sources, it depends on the service-linked role and the rules specified for the role. Do not delete the service-linked role. Otherwise, the use of Beats is affected. For more information, see [Overview of the Elasticsearch service-linked role](/intl.en-US/RAM/Overview of the Elasticsearch service-linked role.md).

4.  Configure and enable Filebeat to collect the logs of the ECS instance.

    1.  In the **Create Shipper** section, move the pointer over **Filebeat** and click **ECS Logs**.

        ![Create Filebeat page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9293895161/p76665.png)

        **Note:** For other shippers on this page, click the shipper name. For example, to create a Metricbeat shipper, click **Metricbeat**.

    2.  In the **Configure Shipper** step, configure the following parameters. Then, click **Next**.

        ![Configure Shipper](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6592640951/p76666.png)

        |Parameter|Description|
        |---------|-----------|
        |**Shipper Name**|The name of the shipper. Enter a name for the shipper. The name must be 1 to 30 characters in length and can contain letters, digits, underscores \(\_\), and hyphens \(-\). The name must start with a letter.|
        |**Version**|Set Version to **6.8.5**, which is the only version supported by Filebeat.|
        |**Output**|The destination for the data collected by Filebeat. The system provides Elasticsearch and Logstash clusters for you to select. If you select Elasticsearch for Output, the protocol must be the same as that of the selected Elasticsearch cluster.|
        |**Username/Password**|If you select **Elasticsearch** for **Output**, enter the username and password used to access the Elasticsearch cluster. This way, Filebeat can write data to the cluster. The default username is elastic. The password is specified when you create the Elasticsearch cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|
        |**Enable Kibana Monitoring**|Determine whether to monitor the metrics of Filebeat. If you select **Elasticsearch** for **Output**, the Kibana monitor uses the same Alibaba Cloud Elasticsearch cluster as **Output**. If you select **Logstash** for **Output**, you must configure a monitor in the configuration file.|
        |**Enable Kibana Dashboard**|Determine whether to enable the default Kibana dashboard. Alibaba Cloud Kibana is configured in a VPC. You must enable the Private Network Access feature for Kibana on the Kibana configuration page. For more information, see [Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure an IP address whitelist for access to the Kibana console over the Internet
         or an internal network.md).|
        |**Filebeat Log File Path**|This parameter is specific to Filebeat. Alibaba Cloud deploys Beats with Docker. You must map the directory from which logs are collected to Docker. We recommend that you specify a directory that is consistent with `input.path` in filebeat.yml.|
        |**Shipper YML Configuration**|Prepare configuration files for the shipper. You can modify the YML configuration files based on your business requirements. For more information, see [Prepare the YML configuration files for a shipper](/intl.en-US/Beats Data Shippers/Prepare the YML configuration files for a shipper.md).|

        **Note:** If you already specify **Output**, you do not need to specify it again in **Shipper YML Configuration**. Otherwise, the system reports a shipper installation error.

    3.  In the **Install Shipper** step, select the ECS instance on which you want to install a shipper.

        ![Install Shipper](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6592640951/p76673.png)

        **Note:** The system displays all the ECS instances within your account that reside in the same VPC as the Elasticsearch or Logstash cluster selected for **Output**. A shipper can be installed only on an ECS instance on which Cloud Assistant and Docker are installed.

    4.  Click **Enable**.

    5.  In the **Enable Shipper** dialog box, click **Back to Beats Shippers**. In the Manage Shippers section of the Beats Data Shippers page, view the newly created shipper.

        After the **Status** of the shipper changes to **Enabled**, the shipper is created. The two numbers following **Enabled** indicate the number of ECS instances on which the shipper is installed and the total number of ECS instances on which you want to install the shipper. If the shipper is installed on all ECS instances, the two numbers are equal.

        ![Status of the effective shipper](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7592640951/p76692.png)

5.  View the running ECS instance.

    After the shipper is created, you can view the running ECS instance to check whether the shipper installation succeeds and handle exceptions as prompted.

    1.  In the **Manage Shippers** section, find the shipper that you created and click **View Instances** in the **Actions** column.

    2.  In the **View Instances** panel, check whether the shipper installation on the ECS instance succeeds.

        The **Installed Shippers** column provides the values **Heartbeat Normal**, **Heartbeat Abnormal**, and **Installation Failed** to indicate whether the shipper installation on an ECS instance succeeds. If the value of Installed Shippers is **Heartbeat Abnormal** or **Installation Failed**, you can remove the instance or retry the installation on the instance. If the retry fails, you can troubleshoot the issue based on the instructions provided in [Installation failures of Beats shippers]().

        ![View the running ECS instance](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7592640951/p76696.png)

    3.  Click **Add Instance** to add the ECS instances on which you want to install a shipper with the same configuration and type as the created shipper.

6.  View monitoring and dashboard information.

    If you select **Enable Kibana Monitoring** or **Enable Kibana Dashboard** in the Configure Shipper step, you can view the monitoring information or dashboard graphs in the Kibana console of your Elasticsearch cluster after Beats is started.

    1.  In the **Manage Shippers** section, find the shipper that you created and choose **More** \> **Dashboard** in the **Actions** column.

    2.  On the logon page of the Kibana console, enter the username and password, and click Log in.

    3.  In the left-side navigation pane, click **Dashboard** and click a metric whose dashboard you want to view. Then, you can view the dashboard of the metric.

        ![View a dashboard](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7592640951/p76699.png)

    4.  In the left-side navigation pane, click **Monitoring** and select a monitoring item whose information you want to view. Then, you can view the information of the monitoring item.

        ![View monitoring information](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7592640951/p76700.png)


