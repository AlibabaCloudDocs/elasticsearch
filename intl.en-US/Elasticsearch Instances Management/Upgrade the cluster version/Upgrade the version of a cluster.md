---
keyword: upgrade the version of an Elasticsearch cluster
---

# Upgrade the version of a cluster

Alibaba Cloud Elasticsearch allows you to upgrade the version or kernel of your Alibaba Cloud Elasticsearch cluster. This ensures that your business is up-to-date. You can upgrade Elasticsearch clusters from V5.6.16 to V6.3.2 or from V6.3.2 to V6.7.0. Upgrades from V5.6.16 to V6.3.2 are unavailable in the Alibaba Cloud Elasticsearch console. If you want to perform such an upgrade, submit a ticket.

-   A precheck is performed for the version upgrade.

    For more information about the related check items, see [Check items for cluster status](#section_qb4_8ob_p09) and [Check for and modify incompatible configurations before you perform an upgrade from V5.6 to V6.3]().

-   Kernel upgrades: A new kernel version is available.

    You can go to the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page of your cluster to check whether a new kernel version is available for the cluster.

    ![Upgrade a kernel](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1467819951/p94038.png)

-   Upgrades from V5.6.16 to V6.3.2: The configurations of your cluster are modified to avoid configuration incompatibility. V6.X may be incompatible with some configurations in V5.X. If these configurations are not modified, the cluster may not properly provide services after the upgrade. For more information about the incompatible configurations and how to modify these configurations, see [Check for and modify incompatible configurations before you perform an upgrade from V5.6 to V6.3]().

    **Note:** For more information about major changes in Elasticsearch V6.X, see [Breaking changes in 6.0](https://www.elastic.co/guide/en/elasticsearch/reference/6.4/breaking-changes-6.0.html).


## Precautions

When you upgrade the version or kernel of your cluster, the system restarts the cluster. The following table describes the restart modes that are provided by Alibaba Cloud Elasticsearch.

|Restart mode|Principle|Risk and recommendation|Scenario|
|------------|---------|-----------------------|--------|
|Blue-green change restart|The system adds nodes of a later version to the cluster, migrates data stored on original nodes to the added nodes, and removes the original nodes from the cluster.|-   An ongoing upgrade cannot be canceled. During the upgrade, you can only read data from or write data to your cluster. Therefore, we recommend that you perform an upgrade during off-peak hours.
-   After the upgrade, the IP addresses of the nodes in the cluster are changed. If you specified the IP addresses in the configuration file of the cluster, update the IP addresses after the upgrade.

|-   Upgrades from V6.3.2 to V6.7.0
-   Kernel upgrades |
|Full restart|The system disables all nodes in the cluster and restarts the cluster.|During an upgrade, the system [installs a TLS certificate](https://www.elastic.co/guide/en/elasticsearch/reference/current/ssl-tls.html) on your cluster. This may interrupt your services but does not cause data loss. Upgrade duration depends on the specifications of the cluster and the volume of data stored on the cluster. We recommend that you plan the upgrade in advance.**Note:** A full restart does not change the IP addresses of the nodes in the cluster. Therefore, you do not need to update the configuration file of the cluster.

|Upgrades from V5.6.16 to V6.3.2|

## Check items for cluster status

Before you upgrade the version or kernel of your cluster, check whether the cluster is in a normal state and whether its load is normal. You can upgrade the version or kernel of your cluster only when the cluster is in a normal state and its load is normal.

|Check item|Normal state|
|----------|------------|
|Cluster status|The cluster is in the Active state \(indicated by the color green\).|
|JVM heap memory usage|The JVM heap memory usage of the cluster is less than 75%.|
|Disk usage|The disk usage of nodes is less than the value of cluster.routing.allocation.disk.watermark.low.|
|Replica shards|All indexes are configured with replica shards.|
|Snapshots|The cluster created snapshots during the last hour.|
|Custom plug-ins|The cluster does not have custom plug-ins installed.|
|ECS instances in the zone where the cluster resides|The zone where the cluster resides has sufficient ECS instances.**Note:** To upgrade the version or kernel of a cluster, add nodes of a later version to the cluster, migrate data stored on original nodes to the added nodes, and remove the original nodes from the cluster. Therefore, before the upgrade, make sure that the zone where the cluster resides has sufficient ECS instances. |
|YML configuration file|The cluster of a later version is compatible with the YML configuration file in an earlier version.|

## Procedure

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the upper-right corner of the **Basic Information** page, click **Update and Upgrade**.

5.  In the **Upgrade** dialog box, select the target version.

    **Note:** A kernel upgrade does not change the version of the cluster. The kernel upgrade entry is displayed only after the system detects a new kernel version. After the kernel is upgraded, the entry is no longer displayed. For more information about the new features that are provided by the upgraded kernel, see [AliES release notes](/intl.en-US/AliES Kernel/AliES release notes.md).

6.  Click **Precheck**.

    Then, the system checks the configuration compatibility, status, snapshots, and basic resources of the cluster.

    ![Upgrade precheck](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0467819951/p77587.png)

    After the check is completed, handle exceptions as prompted. The following description provides specific instructions on the check items:

    -   Configuration compatibility check

        Check whether the later version is compatible with all configurations in the earlier version, especially for major version upgrades such as upgrades from V5.X to V6.X. If the cluster fails the check, the upgrade is terminated. In this case, modify the incompatible configurations based on the instructions provided in [Check for and modify incompatible configurations before you perform an upgrade from V5.6 to V6.3]() and perform the upgrade again.

    -   Cluster status check

        Check whether the cluster is in the Active state \(indicated by the color green\) and whether its load is normal. Before the check or if the cluster fails the check, you can check whether the cluster load is normal based on the instructions provided in [Check items for cluster status](#section_qb4_8ob_p09).

    -   Snapshot check

        Check whether the cluster created snapshots during the last hour before the upgrade. If the cluster has not created snapshots during the last hour, you can click **Create Snapshots** in the **Upgrade** dialog box to trigger snapshot creation

        **Note:** If the upgrade fails, you can restore data from the snapshots. The time that is required to create snapshots depends on the volume of data stored on the cluster. If the Auto Snapshot feature is disabled for the cluster and the data volume is large, a longer time is required to create the first snapshot.

7.  After the cluster passes the precheck, click **Upgrade**.

    During the upgrade, you can view the upgrade progress in the [Tasks](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the progress of a cluster task.md) dialog box.

    After the upgrade, you can view the cluster or kernel version on the **Basic Information** page.


