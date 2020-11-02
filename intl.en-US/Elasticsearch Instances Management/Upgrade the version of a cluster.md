---
keyword: Elasticsearch cluster version upgrade
---

# Upgrade the version of a cluster

This topic describes how to upgrade the version of an Elasticsearch cluster. Currently, you can upgrade an Elasticsearch cluster only from V6.3.2 to V6.7.0 with one click.

A version upgrade check is performed.

For more information about relevant check items, see [Check items before a version upgrade](#section_alr_o6z_g8i).

## Precautions

-   When you upgrade the version of an Elasticsearch cluster, you can continue to read data from or write data to the cluster, but you cannot cancel the upgrade or make other changes. We recommend that you perform a version upgrade during off-peak hours.
-   To upgrade the version of a cluster, add nodes of the target version to the cluster, migrate data stored on nodes of the source version to the new nodes, and remove the nodes of the source version. This causes the IP addresses of nodes to change.

## Check items before a version upgrade

Before you perform a version upgrade, check items listed in the following table. You can only upgrade the versions of Elasticsearch clusters that are in normal states.

|Check item|Normal state|
|----------|------------|
|Cluster state|The cluster is in the Active state \(indicated by the color green\).|
|JVM heap memory usage|The JVM heap memory usage of the cluster is less than 75%.|
|Disk usage|The disk usage of nodes is less than the value of `cluster.routing.allocation.disk.watermark.low`.|
|Replica|All indexes are configured with replicas.|
|Snapshot|The cluster created snapshots within the last hour.|
|Custom plug-in|The cluster does not have custom plug-ins installed.|
|ECS instance in the zone where the cluster resides|The zone where the cluster resides contains sufficient ECS instances. **Note:** To upgrade the version of a cluster, add nodes of the target version to the cluster, migrate data stored on nodes of the source version to the new nodes, and remove the nodes of the source version. Therefore, before the upgrade, ensure that the zone where the cluster resides has sufficient ECS instances. |
|YML file configuration|The cluster of the target version is compatible with the YML file configuration of the source version.|

## Procedure

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the upper-right corner of the **Basic Information** page, click **Update and Upgrade**.

5.  In the **Upgrade** dialog box, select the target version.

6.  Click **Precheck**.

    The system then checks the configuration compatibility, status, snapshots, and basic resources of the cluster.

    ![Upgrade check](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0467819951/p77587.png)

    After the check is complete, handle exceptions as prompted. For example, if the cluster has not created snapshots within the last hour, you can click **Create Snapshots** to trigger the snapshot operation.

7.  After the check is successful, click **Upgrade**.

    During the upgrade, you can view the upgrade progress in the [Tasks](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the progress of a cluster task.md) dialog box.

    After the upgrade, you can view the cluster version on the **Basic Information** page.

    **Note:** After the upgrade, the IP addresses of nodes change. If you specified the IP addresses in the cluster configuration, update them after the upgrade.


