# Create an Alibaba Cloud Logstash cluster

Before you create and configure an Alibaba Cloud Logstash pipeline, you must create a Logstash cluster. This topic describes how to create a Logstash cluster.

-   An Alibaba Cloud account is created.

    To create an Alibaba Cloud account, visit the [account registration](https://account.aliyun.com/register/register.html) page.

-   A virtual private cloud \(VPC\) and a vSwitch are created.

    For more information, see [Create a VPC and a vSwitch](/intl.en-US/Logstash/Quick Start/Preparations.md).

    **Note:** A Logstash cluster can push data only to an Elasticsearch cluster that has the same version and resides in the same VPC.


1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Logstash Clusters**.

3.  On the **Clusters** page, click **Create**.

4.  On the buy page, configure the cluster launch settings.

    In this topic, Billing Method is set to Pay-As-You-Go and Version is set to 6.7. Default values are retained for other parameters. For more information, see [Parameters on the buy page](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Parameters on the buy page.md).

    **Note:**

    -   We recommend that you purchase **pay-as-you-go** Logstash clusters for program development or functional testing.
    -   Discounts are offered for **subscription** clusters.
5.  Read and select **Logstash \(Pay-as-you-go\) Terms of Service**. Then, click **Buy Now**.

6.  After the cluster is created, click **Console** to navigate to the **Clusters** page and view the newly created cluster.


After the state of the cluster changes to **Active**, create and run a data synchronization task. For more information, see [Step 2: Create and run a data synchronization task](/intl.en-US/Logstash/Quick Start/Step 2: Create and run a data synchronization task.md).

