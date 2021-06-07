---
keyword: switch the billing method of an Elasticsearch cluster from pay-as-you-go to subscription
---

# Switch the billing method of a cluster from pay-as-you-go to subscription

After you create a pay-as-you-go Alibaba Cloud Elasticsearch cluster, you can switch its billing method to subscription to pay only for reserved resources. This topic describes the procedure in detail.

The Elasticsearch cluster whose billing method you want to switch must meet the following requirements:

-   The cluster belongs to your Alibaba Cloud account.
-   The cluster does not have unpaid switching orders.

    If an unpaid switching order exists, you must cancel the unpaid order and place another order to switch the billing method.

-   The cluster is in a normal state.

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region.

4.  In the left-side navigation pane, click **Elasticsearch Clusters** again. On the page that appears, find the Elasticsearch cluster whose billing method you want to switch and perform one of the following operations:

    -   In the **Actions** column, choose **More** \> **Switch to Subscription**.
    -   Click the ID of the cluster in the Cluster ID/Name column. On the **Basic Information** page, click **Switch to Subscription**.
5.  On the **Confirm Order** page, select a purchase cycle.

    A cluster is purchased on a monthly basis.

6.  Select the **Elasticsearch \(Subscription\) Agreement of Service** check box and click **Activate**.

7.  Complete the payment as prompted.

    After you pay for the order, the billing method of the cluster is switched from pay-as-you-go to subscription.


