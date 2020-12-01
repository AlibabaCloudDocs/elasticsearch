---
keyword: disable auto-renewal for Logstash clusters
---

# Disable auto-renewal

This topic describes how to disable auto-renewal for Alibaba Cloud Logstash clusters. If you do not want to automatically renew your subscription at the end of the billing cycle, we recommend that you disable auto-renewal. If auto-renewal is enabled, the system automatically deducts fees from your account nine days before your Logstash cluster expires. You can disable auto-renewal at any time before the deduction.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, choose **Billing** \> **Renew**.

3.  Specify **Validity Period**, **Instances**, and **Region** to search for the target Logstash clusters.

4.  Click the **Auto** tab.

5.  Disable auto-renewal.

    |Method|Operation|Description|
    |------|---------|-----------|
    |Enable manual renewal|    -   Enable manual renewal for a Logstash cluster: Find the target cluster and click **Enable Manual Renewal** in the **Actions** column.
    -   Enable manual renewal for multiple Logstash clusters: Select the target clusters and click **Enable Manual Renew** below the cluster list.
|After you enable manual renewal for Logstash clusters, you must manually renew the clusters before they expire. For more information, see [Manually renew a Logstash cluster](/intl.en-US/Logstash/Pricing/Renewal/Manually renew a Logstash cluster.md).|
    |Set as nonrenewal|    -   Set nonrenewal for a Logstash cluster: Find the target cluster and click **Nonrenewal** in the **Actions** column.
    -   Set nonrenewal for multiple Logstash clusters: Select the target clusters and click **Set as Nonrenewal** below the cluster list.
|After you set nonrenewal for Logstash clusters, you can still manually renew the clusters before they expire. However, you are reminded only once of the cluster expiration, and the clusters are immediately suspended after they expire. You can change your renewal configurations at any time before they expire.|

6.  Click **OK**.

    If the managed clusters appear in the list on the **Manual** or **Nonrenewal** tab, auto-renewal has been disabled.


