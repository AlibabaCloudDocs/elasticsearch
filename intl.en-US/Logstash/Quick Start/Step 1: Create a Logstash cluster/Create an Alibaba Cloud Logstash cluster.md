# Create an Alibaba Cloud Logstash cluster

This topic describes how to create an Alibaba Cloud Logstash cluster.

-   An Alibaba Cloud account is created.

    To create an Alibaba Cloud account, click [here](https://account.aliyun.com/register/register.html).

-   A Virtual Private Cloud \(VPC\) and a VSwitch are created.

    For more information, see [Create a VPC and a vSwitch](/intl.en-US/Logstash/Quick Start/Preparations.md).

    **Note:** A Logstash cluster can push data only to an Elasticsearch cluster that has the same version and resides in the same VPC.


1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  On the **Clusters** page, click **Create**.

3.  On the buy page, complete cluster launch configurations.

    In this tutorial, **Billing Method** is set to **Pay-As-You-Go** and **Version** is set to **6.7**. Default values are used for other parameters. For more information about parameters on the buy page, see [Parameters on the buy page](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Parameters on the buy page.md).

    **Note:**

    -   We recommend that you purchase **pay-as-you-go** Logstash clusters for program development or functional tests.
    -   Discounts are offered for **subscription** Logstash clusters.
4.  Click **Next: Confirm Order** to preview the cluster configurations.

    If the cluster configurations do not meet your requirements, click the ![Modify cluster configurations](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0367819951/p84860.png) icon to modify the configurations.

    The following figure shows the cluster configurations in this tutorial.

    ![Logstash cluster configurations](../images/p85383.png)

5.  Read the terms of service and select **I have read and agree to Alibaba Cloud Logstash \(Pay-as-you-go\) Terms of Service**. Then, click **Buy Now**.

6.  After the cluster is created, click **Console** to navigate to the **Clusters** page and view the created cluster.


After the state of the cluster changes to **Active**, create and run a data synchronization task. For more information, see [Step 2: Create and run a data synchronization task](/intl.en-US/Logstash/Quick Start/Step 2: Create and run a data synchronization task.md).

