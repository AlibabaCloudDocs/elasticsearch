---
keyword: release an Elasticsearch cluster
---

# Release a cluster

You can release pay-as-you-go clusters or expired subscription clusters. If a subscription cluster has not expired, you must claim a refund before you release the cluster. This topic describes how to release a pay-as-you-go cluster.

## Precautions

After a cluster is released, data stored on the cluster cannot be restored. We recommend that you back up data before you release a cluster. For more information, see [Create manual snapshots and restore data from manual snapshots](/intl.en-US/Elasticsearch Instances Management/Data backup/Create manual snapshots and restore data from manual snapshots.md).

## Procedure

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region.

4.  On the **Elasticsearch Clusters** page, find the cluster that you want to release and choose **More** \> **Release** in the **Actions** column.

    ![Release a cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9367819951/p97626.png)

5.  In the dialog box that appears, choose whether to select **Immediately Delete** and click **OK**.

    ![Release dialog box](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5673416161/p213112.png)

    -   You choose not to select Immediately Delete.

        In this case, the cluster is released after 24 hours. During this period, you can still find the cluster on the **Elasticsearch Clusters** page and perform the following operations on it:

        -   Choose **More** \> **Recover** in the Actions column to recover the cluster. After confirmation, the cluster is recovered. Then, the system continues to charge you for the cluster.
        -   Choose **More** \> **Delete** in the Actions column to immediately delete the cluster. After confirmation, the cluster is released, and the data stored on the cluster is deleted. In addition, the system removes the cluster from the **Elasticsearch Clusters** page.
    -   You choose to select Immediately Delete.

        In this case, all the data stored on the cluster is deleted, and the system removes the cluster from the **Elasticsearch Clusters** page.


## References

[DeleteInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/DeleteInstance.md)

