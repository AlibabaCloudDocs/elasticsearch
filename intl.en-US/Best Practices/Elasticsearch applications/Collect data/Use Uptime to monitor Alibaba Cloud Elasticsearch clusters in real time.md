# Use Uptime to monitor Alibaba Cloud Elasticsearch clusters in real time

Heartbeat shippers can detect the statuses of network endpoints based on the HTTP or HTTPS, TCP, and ICMP services on a regular basis. They can also send the collected detection data to the Uptime application in Kibana. Then, the Uptime application monitors the availability and response time of applications and services in real time and reports errors before your business is affected. This topic describes how to use Uptime to monitor an Alibaba Cloud Elasticsearch cluster in real time.

Uptime must be used with the following services:

-   Heartbeat
-   Elasticsearch
-   Kibana

**Note:** You can also use the Alerting and Actions feature of Kibana V7.7 to configure monitoring and alerting. For more information, see [Alerting and Actions](https://www.elastic.co/guide/en/kibana/7.7/alerting-getting-started.html#alerting-setup-prerequisites).

## Deployment architecture

-   Deployment of a single Heartbeat shipper

    A single Heartbeat shipper is deployed at a single position to monitor a single service. The Heartbeat shipper sends monitoring data to Elasticsearch. You can view the heartbeat data of the service on the Uptime page of Kibana and determine the service status based on the heartbeat data.

    ![Deployment of a single Heartbeat shipper](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4007422161/p207825.png)

-   Deployment of multiple Heartbeat shippers

    Two Heartbeat shippers are deployed at different positions to monitor the same service. The two Heartbeat shippers send monitoring data to Elasticsearch. You can view the heartbeat data of the service on the Uptime page of Kibana and determine the service status based on the heartbeat data. If the Heartbeat shipper at one position becomes faulty, the Heartbeat shipper at the other position can help locate the fault.

    ![Deployment of multiple Heartbeat shippers](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4007422161/p207826.png)


For more information about the deployment architecture, see [Deployment Architecture](https://www.elastic.co/guide/en/uptime/7.9/uptime-deployment-arch.html).

## Preparations

1.  Create an Alibaba Cloud Elasticsearch cluster and enable the Auto Indexing feature for the cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md) and [Configure the YML file](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md).

2.  Create an Elastic Compute Service \(ECS\) instance, which is used to deploy a Heartbeat shipper. The ECS instance must reside in the same virtual private cloud \(VPC\) as the Elasticsearch cluster.

    For more information, see [Create an instance by using the wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the wizard.md).

    **Note:** Beats supports only the following operating systems: Alibaba Cloud Linux, Red Hat Enterprise Linux \(RHEL\), and Community Enterprise Operating System \(CentOS\). Therefore, you must select one of the preceding operating systems when you create the ECS instance.

3.  Install Cloud Assistant and Docker on the ECS instance.

    For more information, see [Install the Cloud Assistant client](/intl.en-US/Deployment & Maintenance/Cloud assistant/Configure the Cloud Assistant client/Install the Cloud Assistant client.md) and [Deploy and use Docker on Alibaba Cloud Linux 2 instances](/intl.en-US/Tutorials/Build an application/Deploy and use Docker/Deploy and use Docker on Alibaba Cloud Linux 2 instances.md).


## Create a Heartbeat shipper

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  On the Beats Data Shippers page, click **Heartbeat** in the **Create Shipper** section.

3.  Install and configure a Heartbeat shipper.

    For more information, see [Install a shipper](/intl.en-US/Beats Data Shippers/Install a shipper.md) and [Prepare the YML configuration files for a shipper](/intl.en-US/Beats Data Shippers/Prepare the YML configuration files for a shipper.md).

    ![Configure Shipper step](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4007422161/p207864.png)

    |Parameter|Description|
    |---------|-----------|
    |type|In this topic, http is used.**Note:** The Heartbeat shipper can monitor HTTP or HTTPS, TCP, and ICMP services. If you use an HTTP or HTTPS monitor, response code, request bodies, and request headers can be monitored. If you use a TCP monitor, port numbers and strings can be monitored. |
    |urls|The URLs that you want to check. You can specify multiple HTTP services. In this topic, an Alibaba Cloud Elasticsearch cluster is checked. This parameter is set to the internal endpoint of the Elasticsearch cluster that you want to check.|
    |schedule|The interval at which checks are performed. The value @every 10s indicates that the check is performed every 10 seconds.|

4.  Click **Next**.

5.  In the **Install Shipper** step, select the ECS instance on which you want to install the Heartbeat shipper.

    ![Select the ECS instance on which you want to install the Heartbeat shipper](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0657359951/p82419.png)

6.  Enable the Heartbeat shipper and check whether the Heartbeat shipper is installed.

    For more information, see [Install a shipper](/intl.en-US/Beats Data Shippers/Install a shipper.md).

    If the state of the Heartbeat shipper is **Enabled**, and the installation state of the Heartbeat shipper is **Heartbeat Normal**, the Heartbeat shipper is installed.

    ![Shipper installation succeeded](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4007422161/p208030.png)


## View the monitoring information on the Uptime page

1.  Log on to the Kibana console of your Elasticsearch cluster.

    The Kibana console is the one that corresponds to the Elasticsearch cluster that you specified for **Output** when you create the Heartbeat shipper. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Uptime**. On the Uptime page, you can view the monitoring information.

    ![View the monitoring information on the Uptime page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4007422161/p208049.png)

    -   Color red: indicates that the Elasticsearch cluster is in an abnormal state. Check the communication status of the Heartbeat shipper or the status of the Elasticsearch cluster.
    -   Color blue: indicates that the Elasticsearch cluster is in a normal state.

