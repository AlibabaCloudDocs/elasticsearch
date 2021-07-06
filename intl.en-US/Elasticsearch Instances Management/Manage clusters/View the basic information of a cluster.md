# View the basic information of a cluster

You can obtain the basic information, such as the internal or public endpoint, port number, version, and type of an Elasticsearch cluster from the Basic Information page of the cluster. This topic describes the information displayed on this page.

## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  Navigate to the desired cluster.

    1.  In the top navigation bar, select a resource group and a region.

    2.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the **Elasticsearch Clusters** page, find the desired cluster and click its ID.

4.  On the **Basic Information** page, view the basic information and statistics of the cluster.

    ![Basic Information page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8177659951/p59974.png)

    |Section|Parameter|Description|
    |-------|---------|-----------|
    |**Basic Information**|**Cluster ID**|The unique ID of the cluster.|
    |**Name**|By default, the cluster name is the same as the cluster ID. Cluster names are configurable. You can search for clusters by name.|
    |**Version**|The version of the cluster. Valid values: **5.5.3**, **5.6.0**, **6.3.2**, **6.7.0**, **6.8.0**, and **7.4.0**. **Note:** If you want to upgrade the version of the cluster, you can use the version upgrade feature of Elasticsearch. You can upgrade clusters from V5.6.16 to V6.3.2 or from V6.3.2 to 6.7.0. For more information, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade version/Upgrade the version of a cluster.md). |
    |**Cluster Type**|The type of the cluster. Valid values: **Standard** and **Advanced**.|
    |**Region**|The region where the cluster resides.|
    |**Zone**|The zone where the cluster resides.|
    |**VPC ID**|The virtual private cloud \(VPC\) to which the cluster belongs.|
    |**vSwitch ID**|The vSwitch to which the cluster belongs.|
    |**Internal Endpoint**|The internal endpoint of the cluster. In a VPC, you can use an Elastic Compute Service \(ECS\) instance to connect to the internal endpoint of an Elasticsearch cluster. **Note:** For security and stability purposes, we recommend that you do not access an Elasticsearch cluster over the Internet. If you require high security and stability, purchase an ECS instance in the VPC where your Elasticsearh cluster resides. This way, you can use the ECS instance to connect to the internal endpoint of the Elasticsearch cluster. |
    |**Internal Port**|Elasticsearch supports the following ports:     -   Port **9200** for HTTP and HTTPS.
    -   Port **9300** for TCP. Only Elasticsearch V5.X clusters support this port.

**Note:** You cannot use Transport Client to access Elasticsearch clusters of V6.X or later over port 9300. If you want to use port 9300, purchase a V5.X cluster. For more information about how to access a cluster, see [Access and configure an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Access and configure an Elasticsearch cluster.md). |
    |**Public Network Access**|The public endpoint of the cluster. You can use this endpoint to access the cluster over the Internet. Before you use this endpoint, you must turn on Public Network Access on the Security page. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).**Note:** If you use the public endpoint to access the Elasticsearch cluster, you must configure a whitelist to access the cluster over the Internet. By default, all IP addresses cannot be used to access the cluster over the Internet. For more information about how to configure the whitelist, see [Configure a whitelist to access an Elasticsearch cluster over the Internet](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md). |
    |**Public Network Port**|The port used to connect to the cluster over the Internet. This parameter is displayed only after you turn on **Public Network Access**. Elasticsearch supports the following ports:     -   Port **9200** for HTTP and HTTPS.
    -   Port **9300** for TCP. Only Elasticsearch V5.X clusters support this port.

**Note:** You cannot use Transport Client to access Elasticsearch clusters of V6.X or later over port 9300. If you want to use port 9300, purchase a V5.X cluster. For more information about how to access a cluster, see [Access and configure an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Access and configure an Elasticsearch cluster.md). |
    |**Protocol**|The protocol used by the cluster. Default value: **HTTP**. Valid values: **HTTP** and **HTTPS**.|
    |**Tag**|The tag added to the cluster. You can use tags to classify and manage Elasticsearch clusters. For more information, see [Manage cluster tags](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Manage cluster tags.md).|
    |**Cluster Statistics**|**Status**|The status of the cluster. An Elasticsearch cluster has the following states: **Active** \(indicated by the color green\), **Initializing** \(indicated by the color yellow\), **Paused** \(indicated by the color red\), and **Expired** \(indicated by the color gray\).|
    |**Billing Method**|The billing method of the cluster. Valid values: **Pay-As-You-Go** and **Subscription**.|
    |**Created At**|The time at which the cluster is created.|
    |**Maintenance Window**|The maintenance window for the cluster. The default value of this parameter is **02:00 - 06:00**. You can specify **Maintenance Window** based on your business requirements. For more information, see [Set a maintenance window](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Set a maintenance window.md).|

    **Note:**

    -   If you want to renew a subscription cluster, click **Renew** on the right side of the **Basic Information** page to perform the operation. The minimum renewal period of a cluster is one month. For more information, see [t1880497.md\#](/intl.en-US/Pricing/Renewal/Manually renew an Elasticsearch cluster.md).
    -   If you want to change the billing method of a cluster from pay-as-you-go to subscription, click **Switch to Subscription** on the right side of the **Basic Information** page to perform the operation. The **Switch to Subscription** feature allows you to change the billing method of an Elasticsearch cluster from **Pay-As-You-Go** to **Subscription**. However, no discount is offered when you change the billing method. For more information, see [t1882924.md\#](/intl.en-US/Pricing/Change billing methods/Switch the billing method of a cluster from pay-as-you-go to subscription.md).

## References

[DescribeInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/DescribeInstance.md)

