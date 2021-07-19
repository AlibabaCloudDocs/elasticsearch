---
keyword: [overdue payment, cluster suspension or release]
---

# Overdue payments and cluster suspension or release

If you have an overdue payment, the system sends you a notification to remind you to top up your account. If you do not top up your account within a specific period, the system suspends or releases your cluster.

## Instructions

|Cluster type|Pay-as-you-go|Subscription|
|------------|-------------|------------|
|Elasticsearch|-   If you have an overdue payment, the system sends you a notification.

For pay-as-you-go resources, the system automatically deducts fees from your account. If your account balance is less than zero, the system sends you a notification by text message or email.

-   If you do not top up your account within 15 days after an overdue payment occurs, the system suspends your cluster and sends you a suspension notification.
-   The system releases your cluster and sends you a release notification 15 days after the cluster is suspended.

If your account balance is greater than zero within 15 days after your cluster is suspended, you can continue to use the cluster. If your account balance is still less than zero 15 days after your cluster is suspended, the system releases the cluster. After the cluster is released, the data stored on the cluster is permanently deleted and cannot be restored.

**Note:** For clusters that are automatically released due to overdue payments, the system immediately deletes the clusters and the data stored on them. For clusters that are manually released, the system retains the clusters for 24 hours before the system deletes them and the data stored on them. During this period, you can choose to recover or immediately release them. For more information, see [Release a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Release a cluster.md).


|-   The system sends you notifications three days before your cluster expires, one day before your cluster expires, and on the expiration day.
-   The system sends you notifications on the sixth day and fourteenth day after your cluster expires.
-   The system suspends your cluster and sends you a suspension notification 15 days after the cluster expires.
-   The system releases your cluster and sends you a release notification 15 days after the cluster is suspended.

If your account balance is greater than zero within 15 days after your cluster is suspended, you can continue to use the cluster. If your account balance is still less than zero 15 days after your cluster is suspended, the system releases the cluster. After the cluster is released, the data stored on the cluster is permanently deleted and cannot be restored.

**Note:** For clusters that are automatically released due to overdue payments, the system immediately deletes the clusters and the data stored on them. For clusters that are manually released, the system retains the clusters for 24 hours before the system deletes them and the data stored on them. During this period, you can choose to recover or immediately release them. For more information, see [Release a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Release a cluster.md). |
|Logstash|-   If you have an overdue payment, the system sends you a notification.

For pay-as-you-go resources, the system automatically deducts fees from your account. If your account balance is less than zero, the system sends you a notification by text message or email.

-   If you do not top up your account within 24 hours after an overdue payment occurs, the system suspends your cluster.

You can still use your cluster and the system still deducts fees from your account within 24 hours after your account balance is less than zero. After 24 hours, the system suspends the cluster and stops deducting fees from your account.

-   The system releases your cluster seven days after the cluster is suspended.

If your account balance is greater than zero within seven days after your cluster is suspended, you can continue to use the cluster. If your account balance is still less than zero seven days after your cluster is suspended, the system releases the cluster. After the cluster is released, the data stored on the cluster is permanently deleted and cannot be restored.

**Note:** For clusters that are automatically released due to overdue payments, the system immediately deletes the clusters and the data stored on them. For clusters that are manually released, the system retains the clusters for 24 hours before the system deletes them and the data stored on them. During this period, you can choose to recover or immediately release them. For more information, see [Release a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Release a cluster.md).


|-   The system sends you notifications three days before your cluster expires, one day before your cluster expires, and on the expiration day.
-   The system sends you notifications on the sixth day and fourteenth day after your cluster expires.
-   The system suspends your cluster 15 days after the cluster expires.
-   The system releases your cluster 15 days after the cluster is suspended.

If your account balance is greater than zero within 15 days after your cluster is suspended, you can continue to use the cluster. If your account balance is still less than zero 15 days after your cluster is suspended, the system releases the cluster. After the cluster is released, the data stored on the cluster is permanently deleted and cannot be restored.

**Note:** For clusters that are automatically released due to overdue payments, the system immediately deletes the clusters and the data stored on them. For clusters that are manually released, the system retains the clusters for 24 hours before the system deletes them and the data stored on them. During this period, you can choose to recover or immediately release them. For more information, see [Release a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Release a cluster.md). |

