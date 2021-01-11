---
keyword: migrate Elasticsearch nodes
---

# Migrate nodes in a zone

If the zone where your Elasticsearch cluster resides has insufficient Elastic Compute Service \(ECS\) instances for a configuration upgrade, you can migrate the nodes in the zone to another zone before the upgrade.

-   A zone with sufficient resources exists under the current account.

    We recommend that you select new zones from bottom to top in alphabetical order because these zones may have sufficient resources. For example, if both **cn-hangzhou-e** and **cn-hangzhou-h** are available, select **cn-hangzhou-h**. After you migrate nodes to another zone, you must manually update the configuration of your Elasticsearch cluster. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).

-   The cluster is in the Active state.

    You can run the `GET _cat/health?v` command to check the status of your Elasticsearch cluster.


1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  On the **Basic Information** page, click the **Node Visualization** tab. Then, move your pointer to the zone and click **Migrate**.

    ![Migrate nodes to another zone](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8367819951/p77306.png)

5.  In the **Node Migration** dialog box, set **Target Zone** and **VSwitch**.

    |Parameter|Description|
    |---------|-----------|
    |**Target Zone**|The target zone may also have insufficient ECS instances. We recommend that you select new zones from bottom to top in alphabetical order. For example, if both **cn-hangzhou-e** and **cn-hangzhou-h** are available, select **cn-hangzhou-h**.|
    |**VSwitch**|If your Elasticsearch cluster is deployed in one zone, you must specify a new **VSwitch**. If your Elasticsearch cluster is deployed across zones or is an Alibaba Finance Cloud cluster, you do not need to specify a new **VSwitch**.|

    **Note:**

    -   After migration, the IP addresses of the nodes in the cluster change. If you specified the IP addresses in the cluster configuration, update them after the migration.
    -   Node migration triggers a cluster restart, but the cluster can still provide services during the restart. However, this may cause service instability. We recommend that you perform this operation during off-peak hours.
6.  Read and agree the terms of data migration, and click **OK**.

    The system then restarts the cluster. After the cluster is restarted, its nodes are migrated to the target zone.

    After the migration is complete, the zone specified by **Target Zone** is used.


