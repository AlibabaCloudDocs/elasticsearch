# View the basic information of a cluster

You can obtain the basic information, such as internal or public endpoint, port number, version, and type, of an Elasticsearch cluster from the Basic Information page of the cluster. This topic describes the content displayed on this page.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  On the **Basic Information** page, view the **basic information** and **statistics** of the cluster.

    ![Basic Information page](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8177659951/p59974.png)

    |Parameter|Description|
    |---------|-----------|
    |**Cluster ID**|The unique ID of the cluster.|
    |**Name**|The name of the cluster. By default, the name of a cluster is the same as its ID. Cluster names are configurable. You can search for clusters by name.|
    |**Version**|The version of the cluster. Valid values: **5.5.3**, **5.6.0**, **6.3.2**, **6.7.0**, **6.8.0**, and **7.4.0**. **Note:** If you want to upgrade the version of the cluster, you can use the version upgrade feature of Elasticsearch. You can upgrade only Elasticsearch clusters from V6.3.2 to V6.7.0. For more information, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the version of a cluster.md). |
    |**Cluster Type**|The type of the cluster. Valid values: **Standard** and **Advanced**.|
    |**Region**|The region where the cluster resides.|
    |**Zone**|The zone where the cluster resides.|
    |**VPC**|The Virtual Private Cloud \(VPC\) where the cluster resides.|
    |**VSwitch**|The VSwitch to which the cluster belongs.|
    |**Internal Network Address**|In a VPC, you can use an Elastic Compute Service \(ECS\) instance to connect to the internal endpoint of an Elasticsearch cluster. **Note:** For security and stability purposes, we recommend that you do not access an Elasticsearch cluster over the Internet. If you require high security and stability, we recommend that you purchase an ECS instance that resides in the same VPC as your Elasticsearch cluster. You can then use the ECS instance to connect to the **internal endpoint** of the Elasticsearch cluster. |
    |**Internal Network Port**|The port used to connect to the cluster over an internal network. Elasticsearch supports the following ports:     -   Port **9200** for HTTP and HTTPS.
    -   Port **9300** for TCP. Only Elasticsearch V5.X clusters support this port.

**Note:** You cannot use Transport Client to access Elasticsearch clusters of V6.X or later over port 9300. If you want to use port 9300, purchase a V5.X cluster. For more information about how to access a cluster, see [t134284.md\#](/intl.en-US/Quick Start/Step 3: Access a cluster.md). |
    |**Public Network Access**|The public endpoint of the cluster. You can use this endpoint to access the cluster over the Internet. You must turn on the Public Network Access switch on the Security page. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).**Note:** If you use the public endpoint to access the cluster, you must configure a public network whitelist. By default, Elasticsearch denies requests from all IP addresses. For more information about how to configure the whitelist, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md). |
    |**Public Network Port**|The port used to connect to the cluster over the Internet. This parameter is displayed only after you turn on the **Public Network Access** switch. Elasticsearch supports the following ports:     -   Port **9200** for HTTP and HTTPS.
    -   Port **9300** for TCP. Only Elasticsearch V5.X clusters support this port.

**Note:** You cannot use Transport Client to access Elasticsearch clusters of V6.X or later over port 9300. If you want to use port 9300, purchase a V5.X cluster. For more information about how to access a cluster, see [t134284.md\#](/intl.en-US/Quick Start/Step 3: Access a cluster.md). |
    |**Protocol**|The protocol used by the cluster. Default value: **HTTP**. Valid values: **HTTP** and **HTTPS**.|
    |**Tag**|The tag bound to the cluster. You can use tags to classify and manage Elasticsearch clusters. For more information, see [Manage cluster tags](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Manage cluster tags.md).|
    |**Status**|The state of the cluster. An Elasticsearch cluster has the following states: **Active** \(green\), **Initializing** \(yellow\), **Unhealthy** \(red\), **Paused** \(red\), and **Expired** \(gray\).|
    |**Billing Method**|The billing method of the cluster. Valid values: **Pay-As-You-Go** and **Subscription**.|
    |**Created At**|The time when the cluster was created.|
    |**Maintenance Window**|The maintenance window for the cluster. The default value of this parameter is **02:00 - 06:00**. You can set **Maintenance Window** based on your business needs. For more information, see [Set a maintenance window](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Set a maintenance window.md).|
    |**Renew**|This parameter is displayed only when **Billing Method** is **Subscription**.

You can click **Renew** on the right side of **Basic Information** to renew the cluster. The minimum renewal period of a cluster is one month. For more information, see [Manually renew an Elasticsearch cluster](/intl.en-US/Pricing/Elasticsearch cluster renewal/Manually renew an Elasticsearch cluster.md). |
    |**Switch to Subscription**|This parameter is displayed only when **Billing Method** is **Pay-As-You-Go**.

You can click **Switch to Subscription** in the lower-right corner of the **Basic Information** section to change the billing method. The **Switch to Subscription** feature allows you to change the billing method of an Elasticsearch cluster from **Pay-As-You-Go** to **Subscription**. However, no discount is offered when you change the billing method. For more information, see [Switch the billing method from pay-as-you-go to subscription](/intl.en-US/Pricing/Switch the billing method from pay-as-you-go to subscription.md). |


