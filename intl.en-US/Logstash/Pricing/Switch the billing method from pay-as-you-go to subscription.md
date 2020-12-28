---
keyword: switch the billing method of a Logstash cluster from pay-as-you-go to subscription
---

# Switch the billing method from pay-as-you-go to subscription

After you create a pay-as-you-go Alibaba Cloud Logstash cluster, you can switch its billing method to subscription to pay only for reserved resources. This topic describes the procedure in detail.

The Logstash cluster for which you want to switch the billing method must meet the following requirements:

-   The cluster belongs to your Alibaba Cloud account.
-   The cluster does not have unpaid switching orders.

    If an unpaid switching order exists, you must cancel the unpaid order and place another order to switch the billing method.

-   The cluster is in a normal state.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  On the **Clusters** page, find the cluster whose billing method you want to switch.

5.  In the **Actions** column, choose **More** \> **Switch to Subscription**.

6.  On the **Switch to Subscription** page, specify Duration.

    A cluster is purchased on a monthly basis.

7.  Read and select **Logstash \(Subscription\) Terms of Service**. Then, click **Buy Now**.

8.  Complete the payment as prompted.

    After you pay for the order, the billing method is switched from pay-as-you-go to subscription.


