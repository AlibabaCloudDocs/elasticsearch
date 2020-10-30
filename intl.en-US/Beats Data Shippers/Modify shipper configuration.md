---
keyword: configure a shipper
---

# Modify shipper configuration

After you install a shipper, you can modify its configuration on the Beats Data Shippers page of the Elasticsearch console.

A shipper is installed. For more information, see [Install a shipper](/intl.en-US/Beats Data Shippers/Install a shipper.md).

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home). In the left-side navigation pane, click **Beats Data Shippers**.

2.  In the **Manage Shippers** section, find the shipper whose configuration you want to modify and click **Configure** in the Actions column.

3.  Modify the configuration of the shipper based on your requirements and click **Modify**.

    |Parameter|Description|
    |---------|-----------|
    |**Shipper Name**|Enter a name for the shipper. The name must be 1 to 30 characters in length and can contain letters, digits, underscores \(\_\), and hyphens \(-\). The name must start with a letter.|
    |**Version**|Set Version to **6.8.5**, which is the only version supported by Filebeat.|
    |**Output**|Select a destination for the data collected by Filebeat. The destination is the Elasticsearch cluster you created. The protocol must be the same as that of the selected Elasticsearch cluster.|
    |**Username/Password**|If you select **Elasticsearch** for **Output**, enter the username and password for Filebeat to write data to the Alibaba Cloud Elasticsearch cluster.|
    |**Enable Kibana Monitoring**|Used to monitor the metrics of Filebeat. If you select **Elasticsearch** for **Output**, the Kibana monitor uses the same Alibaba Cloud Elasticsearch cluster as **Output**. If you select **Logstash** for **Output**, you must configure a monitor in the configuration file.|
    |**Enable Kibana Dashboard**|Used to enable the default Kibana dashboard. Alibaba Cloud Kibana is configured in a VPC. You must enable private network access for Kibana on the Kibana configuration page. For more information, see [Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure an IP address whitelist for access to the Kibana console over the Internet
         or an internal network.md).|
    |**Filebeat Log File Path**|This parameter is specific to Filebeat. Alibaba Cloud deploys Beats with Docker. You must map the directory from which logs are collected to Docker. We recommend that you specify a directory that is consistent with `input.path` in filebeat.yml.|
    |**Shipper YML Configuration**|Prepare configuration files for the shipper. You can modify the YML configuration files based on your business requirements. For more information, see [Prepare the YML configuration files for a shipper](/intl.en-US/Beats Data Shippers/Prepare the YML configuration files for a shipper.md).|

    **Note:** If you already specify **Output**, you do not need to specify it again in **Shipper YML Configuration**. Otherwise, the system prompts a shipper installation error.

    After you save the modifications, the shipper state changes to Enabling. After the state changes to Enabled, the shipper configuration modification is complete.


