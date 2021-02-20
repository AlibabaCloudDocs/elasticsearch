# High availability

Alibaba Cloud Elasticsearch provides the cross-zone deployment, load balancing, and data backup and restoration features. It also provides various kernel optimization policies to ensure cluster stability. These features and policies ensure comprehensive data reliability and service availability.

## Data backup and restoration

-   Automatic snapshot creation and data restoration from automatic snapshots: Alibaba Cloud Elasticsearch supports automatic creation of snapshots. You can specify the time at which daily snapshots are automatically created based on your business requirements. After automatic snapshots are created, you can restore data from an automatic snapshot created within three days to the original Alibaba Cloud Elasticsearch cluster. For more information, see [Create automatic snapshots and restore data from automatic snapshots](/intl.en-US/Elasticsearch Instances Management/Data backup/Create automatic snapshots and restore data from automatic snapshots.md).
-   Manual snapshot creation and data restoration from manual snapshots: Alibaba Cloud Elasticsearch allows you to manually run a command to create a snapshot for a specific index. Then, you can save the snapshot in an Object Storage Service \(OSS\) bucket in the same region as your Elasticsearch cluster. After the snapshot is created, you can manually run a command to restore the data in the snapshot to the original Elasticsearch cluster. Alternatively, you can restore the data to an Elasticsearch cluster in the same region as the original Elasticsearch cluster. For more information, see [Commands to create snapshots and restore data](/intl.en-US/Elasticsearch Instances Management/Data backup/Commands to create snapshots and restore data.md).
-   Shared OSS repositories: Alibaba Cloud Elasticsearch allows you to configure shared OSS repositories for your Elasticsearch cluster. This way, you can restore data from the automatic snapshots of an Elasticsearch cluster that are stored in these repositories to your Elasticsearch cluster. For more information, see [Configure a shared OSS repository](/intl.en-US/Elasticsearch Instances Management/Data backup/Configure a shared OSS repository.md).

## AliES enhancements

The Alibaba Cloud Elasticsearch team develops and optimizes the Elasticsearch kernel based on extensive experience to improve cluster stability and availability. The following features are provided by the optimized kernel:

-   [Pruning for time series indexes](/intl.en-US/AliES Kernel/Use the pruning feature for a time series index.md)

    When you query data from a time series index, you can specify a time range to filter the data. This feature improves query performance.

-   [Slow query isolation](/intl.en-US/AliES Kernel/Use the slow query isolation feature.md)

    This feature allows you to track the overheads for a single query request and logically isolate the request. This reduces the impact of anomalous queries on cluster stability.

-   [gig plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the gig plug-in.md)

    When an exception occurs in a cluster, the gig plug-in can perform a switchover within seconds. This reduces the probability that an anomalous node causes query jitters.


For information about other features provided by the optimized kernel, see [AliES release notes](/intl.en-US/AliES Kernel/AliES release notes.md).

## Load balancing

Alibaba Cloud Elasticsearch supports load balancing. You can specify the public or internal endpoint of your Alibaba Cloud Elasticsearch cluster on your application for access. Your requests are evenly distributed to all the data nodes in your Alibaba Cloud Elasticsearch cluster to achieve load balancing.

**Note:** Load balancing among these data nodes depends on the number and size of index shards. When you create indexes, you must set the number and size of index shards to appropriate values. For more information, see [Shard evaluation]().

## Cross-zone deployment

Alibaba Cloud Elasticsearch allows you to deploy an Elasticsearch cluster across zones. In cross-zone deployment, the system automatically selects the zones. If replica shards are configured and nodes in one zone fail, the nodes in the remaining zones can still provide services without interruption. This significantly enhances the availability of the cluster. In addition, you can perform a switchover in the console to isolate the faulty nodes. During the switchover, the system adds computing resources to the remaining zones to make up for the resources lost in the zone that contains the faulty nodes. After the nodes recover, you can perform a recovery for the zone in the Elasticsearch console. During the recovery, the system adds the nodes that were removed during the switchover to the zone again. It also removes the computing resources that were added to the remaining zones during the switchover. The switchover and recovery are imperceptible to customer services, which improves service stability. For more information, see [t1872895.md\#](/intl.en-US/Elasticsearch Instances Management/Precautions.md).

