---
keyword: kernel update
---

# Update the kernel of a cluster

This topic describes how to update the kernel of an Alibaba Cloud Elasticsearch cluster to support the new features developed based on the open-source Elasticsearch kernel. Only the kernels of Alibaba Cloud Elasticsearch V6.7 clusters can be updated.

-   A new kernel version is available.

    You can go to the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page of a cluster to check whether a new kernel version is available for the cluster.

    ![Update a kernel](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1467819951/p94038.png)

-   A check is performed for the kernel update.

    For more information about relevant check items, see [t1854119.md\#](/intl.en-US/Elasticsearch Instances Management/Upgrade/Upgrade the version of a cluster.md).


1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the upper-right corner of the **Basic Information** page, click **Update and Upgrade**.

    **Note:** Updating a kernel does not change the version of the cluster. For more information, see [AliES release notes](/intl.en-US/AliES Kernel/AliES release notes.md).

5.  In the **Upgrade** dialog box, select the target version.

    ![Update a kernel](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1467819951/p94042.png)

    **Note:** When you update the kernel for the first time, the value of **Current Version** is **None** by default.

6.  Click **Precheck**.

    The system then checks the configuration compatibility, status, snapshots, and basic resources of the cluster.

    ![Update a kernel](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1467819951/p94040.png)

    After the check is complete, handle exceptions as prompted. For example, if the cluster has not created snapshots within the last hour, you can click **Create Snapshots** to trigger the snapshot operation.

7.  After the check is successful, click **Upgrade**.

    During the kernel update, you can view the update progress in the [Tasks](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the progress of a cluster task.md) dialog box.

    After the kernel is updated, the **A new kernel patch is available.** message next to **Version** and the **Update and Upgrade** button are no longer displayed.


