---
keyword: switch the billing method from pay-as-you-go to subscription
---

# Switch the billing method from pay-as-you-go to subscription

This topic describes how to switch the billing method of an Alibaba Cloud Elasticsearch cluster from pay-as-you-go to subscription. After you create a pay-as-you-go cluster, you can switch its billing method to subscription to pay only for the reserved resources.

The Elasticsearch cluster for which you want to switch the billing method must meet the following requirements:

-   The cluster belongs to your Alibaba Cloud account.
-   There is no unpaid switch order for the cluster.

    If there is an unpaid switch order, you must cancel the unpaid order and then place another order to switch the billing method.

-   The cluster is properly running.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and perform one of the following operations:

    -   Click **More** and select **Switch to Subscription** in the **Actions** column.
    -   Click its ID in the **Cluster ID/Name** column. On the **Basic Information** page, click **Switch to Subscription**.
4.  On the **Confirm Order** page, select the purchase cycle.

    Elasticsearch allows you to purchase a cluster on a monthly basis. You must select at least one month for the purchase cycle.

5.  Select the **Elasticsearch \(Subscription\) Agreement of Service** check box and click **Activate**.

6.  Complete the payment as prompted.

    The billing method is switched from pay-as-you-go to subscription.


