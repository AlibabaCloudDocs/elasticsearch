---
keyword: [overdue payment, cluster release]
---

# Overdue payments and cluster release

If you have overdue payments, the system sends you notifications to remind you to top up your account. If you do not top up your account within a specific period, the system suspends or releases your cluster.

## Instructions

|Cluster type|Pay-as-you-go|Subscription|
|------------|-------------|------------|
|Elasticsearch|-   The system sends you notifications on the eighth day, twelfth day, and fourteenth day after a payment becomes overdue.
-   If you do not top up your account within 15 days, the system suspends your Elasticsearch cluster and sends you a suspension notification.
-   The system sends you a notification on the sixth day after your Elasticsearch cluster is suspended.
-   The system releases your Elasticsearch cluster and sends you a release notification 15 days after the cluster is suspended. After the cluster is released, the data in the cluster is permanently deleted and cannot be recovered.

|-   The system sends you notifications on the fourteenth day, twelfth day, and eighth day before your Elasticsearch cluster expires.
-   The system suspends your Elasticsearch cluster and sends you a suspension notification 15 days after the cluster expires.
-   The system sends you notifications on the eighth day, twelfth day, and fourteenth day after your Elasticsearch cluster is suspended.
-   The system releases your Elasticsearch cluster and sends you a release notification 15 days after the cluster is suspended. After the cluster is released, the data in the cluster is permanently deleted and cannot be recovered. |
|Logstash|-   The system sends you notifications on the eighth day, twelfth day, and fourteenth day after a payment becomes overdue.
-   The system suspends your Logstash cluster if you do not top up your account within 15 days after a payment becomes overdue.
-   The system releases your Logstash cluster seven days after the cluster is suspended. After the cluster is released, the data in the cluster is permanently deleted and cannot be recovered.
-   The system sends you a release notification nine days before it releases your Logstash cluster.

|-   The system sends you notifications on the seventh day, third day, and first day before your Logstash cluster expires.
-   The system suspends your Logstash cluster 15 days after the cluster expires.
-   The system releases your Logstash cluster one day after the cluster is suspended. After the cluster is released, the data in the cluster is permanently deleted and cannot be recovered.
-   The system sends you notifications on the seventh day, third day, and first day before it releases your Logstash cluster. |

