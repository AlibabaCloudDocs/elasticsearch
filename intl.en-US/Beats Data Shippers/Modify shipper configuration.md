---
keyword: configure a shipper
---

# Modify shipper configuration

After you install a shipper, you can modify its configuration on the Beats Data Shippers page of the Elasticsearch console.

A shipper is installed. For more information, see [Collect the logs of an ECS instance](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md). In this topic, a Filebeat shipper is used.

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the **Manage Shippers** section, find the Filebeat shipper whose configurations you want to modify and click **Configure** in the Actions column.

3.  In the Configure Shipper panel, click **Modify**. Then, modify the configurations of the Filebeat shipper based on your business requirements.

    |Parameter|Description|
    |---------|-----------|
    |**Output**|The destination for the data collected by Filebeat. The system provides Elasticsearch and Logstash clusters for you to select. If you select Elasticsearch for Output, the protocol must be the same as that of the selected Elasticsearch cluster.|
    |**Username/Password**|If you select **Elasticsearch** for **Output**, enter the username and password used to access the Elasticsearch cluster. This way, Filebeat can write data to the cluster. The default username is elastic. The password is specified when you create the Elasticsearch cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|
    |**Enable Kibana Monitoring**|Determine whether to monitor the metrics of Filebeat. If you select **Elasticsearch** for **Output**, the Kibana monitor uses the same Alibaba Cloud Elasticsearch cluster as **Output**. If you select **Logstash** for **Output**, you must configure a monitor in the configuration file.|
    |**Enable Kibana Dashboard**|Determine whether to enable the default Kibana dashboard. Alibaba Cloud Kibana is configured in a VPC. You must enable the Private Network Access feature for Kibana on the Kibana configuration page. For more information, see [Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure an IP address whitelist for access to the Kibana console over the Internet
         or an internal network.md).|
    |**Filebeat Log File Path**|This parameter is specific to Filebeat. Alibaba Cloud deploys Beats with Docker. You must map the directory from which logs are collected to Docker. We recommend that you specify a directory that is consistent with `input.path` in filebeat.yml.|
    |**Shipper YML Configuration**|Prepare configuration files for the shipper. You can modify the YML configuration files based on your business requirements. For more information, see [Prepare the YML configuration files for a shipper](/intl.en-US/Beats Data Shippers/Prepare the YML configuration files for a shipper.md).|

    **Note:** If you already specify **Output**, you do not need to specify it again in **Shipper YML Configuration**. Otherwise, the system reports a shipper installation error.

4.  Click **Save**.

    After you save the modifications, the shipper state changes to Enabling. After the state changes to Enabled, the shipper configuration modification is complete.


