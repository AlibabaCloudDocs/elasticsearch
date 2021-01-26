---
keyword: create an Elasticsearch cluster
---

# Create a cluster

This topic describes how to create an Alibaba Cloud Elasticsearch cluster.

-   An Alibaba Cloud account is created.

    To create an Alibaba Cloud account, visit the [account registration page](https://account.aliyun.com/register/register.html).

-   A virtual private cloud \(VPC\) and a vSwitch are created.

    For more information, see [Create an IPv4 VPC network](/intl.en-US/Quick Start/Create an IPv4 VPC network.md).


## Procedure

1.  Go to the [Elasticsearch buy page](https://common-buy-intl.alibabacloud.com/?spm=a2796.11423565.3381327530.3.6a9a32c2emlY47&commodityCode=elasticsearch_intl#/buy).

2.  On the buy page, configure cluster launch settings.

    For more information, see [Parameters on the buy page](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Parameters on the buy page.md).

    **Note:**

    -   We recommend that you create a pay-as-you-go cluster for program development or functional testing.
    -   Discounts are offered for subscription clusters.
3.  Click **Buy Now**. On the Confirm Order page, read and select the Elasticsearch terms of service. Then, click Pay.

4.  After the cluster is created, click **Console** and go to the **Overview** page of the Elasticsearch console.

5.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the **Clusters** page, view the newly created cluster.

6.  Click the cluster ID. In the upper-right corner of the **Basic Information** page, click the ![Tasks icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5480461161/p203761.png) icon to view the cluster creation progress.

    **Note:**

    -   The cluster can provide services after a period of time. The period of time depends on the specifications, data structure, and data volume of the cluster. In most cases, a few hours are required.
    -   If the cluster information is not promptly updated, you can click **Refresh** in the upper-right corner of the **Basic Information** page to manually update the cluster information. For example, a cluster is in the Failed state after it is created. In this case, you can click Refresh to manually update the status.

## References

[createInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/createInstance.md)

