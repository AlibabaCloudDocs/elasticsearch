# Release a cluster

You can directly release pay-as-you-go clusters or expired subscription clusters. If you want to release a subscription cluster that does not expire, you must request a refund before you release the cluster. This topic describes how to release a pay-as-you-go cluster.

## Precautions

After a cluster is released, you cannot restore the data stored on the cluster. Exercise caution when you release clusters.

## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select a region.

3.  In the left-side navigation pane, click **Logstash Clusters**.

4.  On the Logstash Clusters page, find the cluster that you want to release and choose **More** \> **Release** in the **Actions** column.

    ![Release a cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9252666261/p289658.png)

5.  In the dialog box that appears, select **Immediately Delete** or leave it unselected, and click **OK**.

    ![Release dialog box](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9252666261/p289659.png)

    -   Leave Immediately Delete unselected.

        In this case, the cluster is released 24 hours later. During this period, you can still find the cluster on the Logstash Clusters page and perform the following operations on it:

        -   Choose **More** \> **Recover** in the Actions column to recover the cluster. After you confirm this operation, the cluster is recovered. Then, the system continues to charge you for the cluster.
        -   Choose **More** \> **Delete** in the Actions column to immediately delete the cluster. After you confirm this operation, the cluster is released, and the data stored on the cluster is deleted. In addition, the system removes the cluster from the **Logstash Clusters** page.
    -   Select Immediately Delete.

        In this case, all the data stored on the cluster is deleted, and the system removes the cluster from the **Logstash Clusters** page.


## References

[DeleteLogstash](/intl.en-US/API Reference/Logstash/Manage clusters/DeleteLogstash.md)

