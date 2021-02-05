---
keyword: [Elasticsearch disk space evaluation, Elasticsearch node specification evaluation, shard evaluation]
---

# Evaluate specifications and storage capacity

Before you use Alibaba Cloud Elasticsearch, you must evaluate the total amount of the required resources, such as the disk space, node specifications, number of shards, and size of each shard. Based on test results and user feedback, Alibaba Cloud offers some common methods for the evaluation. You can purchase a cluster or upgrade the configuration of a cluster based on the evaluation results.

## Precautions

-   Different users may have different requirements on data structure, query complexity, data volume, performance, and data changes. This topic is used for reference only. We recommend that you measure the specifications and storage capacity for your Elasticsearch cluster based on actual data and business scenarios.
-   If you purchase a cluster before evaluating its specifications and storage capacity, we recommend that you enable the auto scaling feature for the cluster. This feature allows you to resize disks, add nodes, and upgrade nodes based on evaluation results at all times.

## Supported disk types

This topic is suitable for Elasticsearch clusters that use standard SSDs.

## Disk space evaluation

The disk space of an Elasticsearch cluster is determined by the following factors:

-   Number of replica shards: Each primary shard must have at least one replica shard.
-   Indexing overheads: In most cases, indexing overheads are 10% greater than those of source data. The overheads of the `_all` parameter are not included.
-   Space reserved by the operating system: By default, the operating system reserves 5% of disk space for critical processes, system recovery, and disk fragments.
-   Elasticsearch overheads: Elasticsearch reserves 20% of disk space for internal operations, such as segment merging and logging.
-   Security threshold overheads: Elasticsearch reserves at least 15% of disk space as the security threshold.

Based on these factors, the minimum required disk space is calculated by using the following formula:

Minimum required disk space = Volume of source data × \(1 + Number of replica shards\) × Indexing overheads/\(1 - Linux reserved space\)/\(1 - Elasticsearch overheads\)/\(1 - Security threshold overheads\)

= Volume of source data × \(1 + Number of replica shards\) × 1.7

= Volume of source data × 3.4

**Note:**

-   We recommend that you disable the `_all` parameter unless it is required by your business.
-   Indexes that have the `_all` parameter enabled incur larger overheads on disk usage. Based on test results and user feedback, we recommend that you evaluate the disk space by 1.5 times the original value. This indicates that the minimum required disk space is calculated by using the following formula:

    Minimum required disk space = Volume of source data × \(1 + Number of replica shards\) × 1.7 × \(1 + 0.5\) = Volume of source data × 5.1

-   For an Elasticsearch V6.7 or V7.4 cluster of the Standard Edition, an ultra disk can offer a maximum storage space of 20 TiB for a single node. When you purchase an Elasticsearch cluster, you can specify the storage space as required. Before you resize the disk for an Elasticsearch V6.7.0 cluster, make sure that the kernel of the cluster is updated. For more information about how to update a kernel, see [Update the kernel of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Update the kernel of a cluster.md). For information about how to resize a disk, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).

## Node specification evaluation

The performance of an Elasticsearch cluster is determined by the specifications of each node in the cluster. Based on test results and user feedback, we recommend that you determine node specifications based on the following rules:

-   Maximum number of nodes per cluster: Maximum number of nodes per cluster = Number of vCPUs per node × 5
-   Maximum volume of data per node:

    The maximum volume of data that a node in an Elasticsearch cluster can store depends on the scenario. Examples:

    -   Acceleration or aggregation on data queries: Maximum volume of data per node = Memory size per node \(GiB\) × 10
    -   Log data import or offline analytics: Maximum volume of data per node = Memory size per node \(GiB\) × 50
    -   General scenarios: Maximum volume of data per node = Memory size per node \(GiB\) × 30

The following table lists some node specifications.

|Specification|Maximum number of nodes|Maximum disk space per node in query scenarios|Maximum disk space per node in logging scenarios|Maximum disk space per node in general scenarios|
|-------------|-----------------------|----------------------------------------------|------------------------------------------------|------------------------------------------------|
|2 vCPUs and 4 GiB of memory|10|40 GiB|200 GiB|120 GiB|
|2 vCPUs and 8 GiB of memory|10|80 GiB|400 GiB|240 GiB|
|4 vCPUs and 16 GiB of memory|20|160 GiB|800 GiB|480 GiB|
|8 vCPUs and 32 GiB of memory|40|320 GiB|1.5 TiB|960 GiB|
|16 vCPUs and 64 GiB of memory|80|640 GiB|2 TiB|1.9 TiB|

## Shard evaluation

The number of shards and the size of each shard determine the stability and performance of an Elasticsearch cluster. You must properly plan shards for all indexes in an Elasticsearch cluster. This prevents numerous shards from affecting cluster performance when it is difficult to define business scenarios.

**Note:** In versions earlier than Elasticsearch V7.X, one index has five primary shards and each primary shard has one replica shard by default. In Elasticsearch V7.X and later, one index has one primary shard and the primary shard has one replica shard by default.

Before you plan shards, take note of the following items:

-   Volume of data stored on each index
-   Whether the volume will increase
-   Node specifications
-   Whether you will delete or merge temporary indexes on a regular basis

Based on the preceding items, Alibaba Cloud provides the following guidelines for you to plan shards. These guidelines are for reference only.

-   Before you allocate shards, evaluate the volume of data that you want to store. If the total data volume is large, write a small amount of data to reduce the workloads of your Elasticsearch cluster. In this case, configure multiple primary shards for each index and one replica shard for each primary shard. If both the total data volume and the volume of data that you want to write are small, configure one primary shard for each index and one or more replica shards for each primary shard.
-   Make sure that the size of each shard is no more than 30 GB. In special cases, the size can be no more than 50 GB. If the evaluated data volume exceeds the limit, properly allocate shards before creating indexes and reindex data for these indexes in the future. Reindexing data ensures the normal running of your Elasticsearch cluster but is time-consuming.

    **Note:** If the evaluated data volume is less than 30 GB, you can configure one primary shard for each index and multiple replica shards for each primary shard. This achieves load balancing. For example, the size of each index is 20 GB and your Elasticsearch cluster contains five data nodes. In this case, you can configure one primary shard for each index and four replica shards for each primary shard.

-   In log analytics scenarios or scenarios that require extremely large indexes, make sure that the size of each shard is no more than 100 GB.
-   The total number of primary shards and replica shards is the same as or a multiple of the number of data nodes.

    **Note:** The more primary shards, the more performance overheads your Elasticsearch cluster incurs.

-   Configure a maximum of five shards for each index on a node.
-   Calculate the number of shards for all indexes on a single node by using one of the following formulas:

    -   For clusters with small specifications: Number of shards on a single data node = Memory size of the data node × 30
    -   For clusters with large specifications: Number of shards on a single data node = Memory size of the data node × 50
    **Note:**

    When you calculate the number of shards, you must also take data volume into account. If the data volume is less than 1 TB, we recommend that you calculate the number of shards by using the formula for clusters with small specifications.

    By default, the maximum number of shards on a single node in an Elasticsearch V7.X cluster is 1,000. We recommend that you do not change the maximum number. If you want to change the number of shards on a single node, you can expand node capacity and change the number before you use the cluster.

-   Add at least two independent client nodes. The ratio of client nodes to data nodes must be 1:5, and the vCPU-to-memory ratio of each client node must be 1:4 or 1:8. For example, your Elasticsearch cluster contains 10 data nodes and each data node offers 8 vCPUs and 32 GiB of memory. In this case, you can configure two independent client nodes, each of which offers 8 vCPUs and 32 GiB of memory.

    **Note:** After you use independent client nodes, you can perform a reduce operation on the evaluation result. In this case, if severe garbage collection \(GC\) occurs in the reduce stage, data nodes cannot be affected.

-   If the Auto Indexing feature is enabled, enable [index lifecycle management](/intl.en-US/Best Practices/Elasticsearch applications/Index management/Use ILM to manage Heartbeat indexes.md) or call an Elasticsearch API operation to delete automatically created indexes.
-   Delete small indexes in a timely manner to free up heap memory.

