---
keyword: release an Elasticsearch cluster
---

# Release a cluster

You can directly release pay-as-you-go clusters or expired subscription clusters. If you want to release a subscription cluster that does not expire, you must request a refund before you release the cluster. This topic describes how to release a pay-as-you-go cluster.

## Precautions

After a cluster is released, you cannot restore the data stored on the cluster. We recommend that you back up data before you release a cluster. For more information, see [Create manual snapshots and restore data from manual snapshots](/intl.en-US/Elasticsearch Instances Management/Data backup/Create manual snapshots and restore data from manual snapshots.md).

## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region.

4.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the Elasticsearch Clusters page, find the cluster that you want to release and choose **More** \> **Release** in the **Actions** column.

    ![Release a cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9367819951/p97626.png)

5.  In the dialog box that appears, select **Immediately Delete** or leave it unselected, and click **OK**.

    ![Release dialog box](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5673416161/p213112.png)

    -   Leave Immediately Delete unselected.

        In this case, the cluster is released 24 hours later. During this period, you can still find the cluster on the **Elasticsearch Clusters** page and perform the following operations on it:

        -   Choose **More** \> **Recover** in the Actions column to recover the cluster. After you confirm this operation, the cluster is recovered. Then, the system continues to charge you for the cluster.
        -   Choose **More** \> **Delete** in the Actions column to immediately delete the cluster. After you confirm this operation, the cluster is released, and the data stored on the cluster is deleted. In addition, the system removes the cluster from the **Elasticsearch Clusters** page.
    -   Select Immediately Delete.

        In this case, all the data stored on the cluster is deleted, and the system removes the cluster from the **Elasticsearch Clusters** page.


## References

[DeleteInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/DeleteInstance.md)

