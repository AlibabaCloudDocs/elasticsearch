# Elasticsearch renewal

Subscription Elasticsearch clusters are automatically released after they expire. If you want to continue using the clusters, you can manually renew the clusters or enable auto-renewal for them before the system releases them. This topic describes how to manually renew a subscription Elasticsearch cluster and how to enable or disable auto-renewal for a subscription Elasticsearch cluster.

The cluster that you want to renew is in the Active or Locked state.

## Manually renew a cluster

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  On the **Elasticsearch Clusters** page, find the cluster that you want to renew and perform one of the following operations:

    -   In the **Actions** column, choose **More** \> **Renew**.
    -   In the Cluster ID/Name column, click the cluster ID. In the lower-right corner of the **Basic Information** page, click **Renew**.
4.  On the **Renew** page, specify **Duration**.

    You can renew a cluster on a monthly basis.

5.  Read the terms of service and select **Elasticsearch \(Subscription\) Terms of Service**. Then, click **Buy Now**.

6.  Complete the payment as prompted.

    After you complete the payment, the cluster is automatically renewed.


## Enable auto-renewal for a cluster

If you enable auto-renewal for clusters, you do not need to manually renew the clusters on a regular basis. Auto-renewal also helps prevent service interruptions that occur if you fail to renew clusters in time. You can perform one of the following operations to enable auto-renewal for a cluster:

**Note:** A subscription cluster has a validity period. If you do not renew a subscription cluster before the end of the validity period, service interruptions or even data loss may occur. Pay-as-you-go clusters never expire and do not require renewal.

-   Enable auto-renewal on the buy page

    You can enable auto-renewal on the buy page of a cluster, as shown in the following figure. For more information about how to create a cluster, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

    ![Enable auto-renewal on the buy page](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1230771951/p96431.png)

-   Enable auto-renewal on the Renewal page
    1.  In the top navigation bar of the Elasticsearch console, choose **Expenses** \> **Renewal Management**.
    2.  Specify the Validity Period, Instances, and Region parameters to search for the clusters for which you want to enable auto-renewal.
    3.  On the **Manual** tab, enable auto-renewal for one or more clusters.
        -   Enable auto-renewal for one cluster: Find the cluster and click **Enable Auto Renewal** in the **Actions** column.
        -   Enable auto-renewal for multiple clusters: Select the clusters and click **Enable Auto Renewal** below the cluster list.
    4.  In the Enable Auto Renewal dialog box, specify Unified Auto Renewal Cycle and click **Auto Renew**.

        If the clusters appear in the list on the **Auto** tab, auto-renewal is enabled for the clusters.


## Disable auto-renewal for a cluster

If you enable auto-renewal for your cluster, the system automatically deducts fees from your account nine days before the cluster expires. If you no longer require a cluster after the current billing cycle, you can disable auto-renewal for the cluster before the system deducts fees for the next billing cycle.

1.  On the **Renewal** page, specify the Validity Period, Instances, and Region parameters to search for the clusters for which you want to disable auto-renewal.

2.  Click the **Auto** tab.

3.  Disable auto-renewal.

    |Method|Operation|Description|
    |------|---------|-----------|
    |Enable manual renewal|    -   Enable manual renewal for one cluster: Find the cluster and click **Enable Manual Renewal** in the **Actions** column.
    -   Enable manual renewal for multiple clusters: Select the clusters and click **Enable Manual Renewal** below the cluster list.
|After you enable manual renewal for a cluster, you must manually renew the cluster before it expires. For more information, see [Manually renew a cluster](#section_xdr_naj_cku).|
    |Enable non-renewal|    -   Enable non-renewal for one cluster: Find the cluster and click **Nonrenewal** in the **Actions** column.
    -   Enable non-renewal for multiple clusters: Select the clusters and click **Set as Nonrenewal** below the cluster list.
|After you enable non-renewal for a cluster, the cluster no longer provides services after it expires, and you are reminded of the cluster expiration only once. If you want to continue using the cluster, you can manually renew the cluster or modify its renewal settings before it expires.|

4.  Click **OK**.

    If the clusters appear in the list on the **Manual** or **Nonrenewal** tab, auto-renewal is disabled for the clusters.


