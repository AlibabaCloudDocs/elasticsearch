---
keyword: upgrade the configuration of an Elasticsearch cluster
---

# Upgrade the configuration of a cluster

As your business develops, you may have higher requirements for the configuration of your Elasticsearch cluster. If the current configuration of your Elasticsearch cluster cannot meet your business requirements, you can upgrade the configuration. This topic describes how to upgrade the configuration of an Elasticsearch cluster and the related precautions.

The following operations are performed:

-   Evaluate the specifications and storage capacity of your cluster.

    For more information, see [Evaluate specifications and storage capacity]().

-   Make sure that the specifications of your cluster before and after the upgrade meet the requirements for Elastic Compute Service \(ECS\) instance type changes.

    For more information, see [Instance families that support instance type changes](/intl.en-US/Instance/Change configurations/Instance families that support instance type changes.md).

-   [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) to check whether your cluster stores indexes in the close state. If your cluster stores such indexes, you must open or delete the indexes. Otherwise, the upgrade fails.
    -   Run the following command to view the statuses of indexes:

        ```
        GET /_cat/indices?v
        ```

        ![View the statuses of indexes](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5135141261/p244657.png)

    -   Run the following command to open an index in the close state:

        ```
        POST /<index_name>/_open
        ```

    -   Run the following command to delete an index in the close state:

        **Warning:** Proceed with caution because deleted indexes cannot be recovered.

        ```
        DELETE /<index_name>
        ```


## Precautions

-   Specification upgrades

    You can upgrade the configuration for only one type of node during each upgrade. The node types include data nodes, warm nodes, client nodes, dedicated master nodes, Kibana nodes, and elastic nodes.

    **Note:** You can remove data nodes from your cluster to downgrade the configuration of the cluster. For more information about how to remove data nodes, see [Scale in a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Scale in a cluster.md).

-   Impact on services
    -   After you upgrade the configuration of your cluster, the system restarts the cluster to make the changes take effect. The time required for the restart depends on the specifications, data structure, and data volume of the cluster. In most cases, the restart requires a few hours. For more information, see [Restart a cluster or node](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Restart a cluster or node.md).

        **Note:**

        -   In most cases, if the indexes of your cluster have replica shards and the load of your cluster is normal, your cluster can still provide services during a cluster configuration change. The following items indicate that the load of a cluster is normal: CPU utilization is about 60%, heap memory usage is about 50%, and the value of NodeLoad\_1m is less than the number of vCPUs.
        -   If the indexes of your cluster do not have replica shards, the load of the cluster is excessively high, and large amounts of data are written to or queried in your cluster, access to the cluster may time out during a cluster configuration change. We recommend that you configure an access retry mechanism for your client before you perform a cluster configuration change. This reduces the impact on your business.
    -   If your cluster is abnormal \(indicated by the color yellow or red\), you must select **Forced Update**. This may affect services.
-   Updates of cloud disk types

    Cloud disks with low storage performance can be updated to cloud disks with high storage performance. The following types of cloud disks are listed in ascending order of their storage performance: ultra disks, standard SSDs, and enhanced SSDs. You can update the cloud disk type of your cluster based on your business requirements. For more information about cloud disks, see [Cloud disks](/intl.en-US/Block Storage/Block Storage overview/Cloud disks.md).

-   Version upgrades

    You cannot upgrade the version of your cluster during a configuration upgrade. For more information, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade version/Upgrade the version of a cluster.md).

    **Note:** If you perform a version upgrade during a configuration upgrade, the system displays the "UpgradeVersionMustFromConsole" error message.

-   Changes in billing

    After you submit a configuration upgrade order, your cluster is charged based on the new configuration.

    **Note:** During a configuration upgrade, you can check the price of your order on the **Upgrade/Downgrade** page in real time.


## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  Navigate to the desired cluster.

    1.  In the top navigation bar, select a resource group and a region.

    2.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the **Elasticsearch Clusters** page, find the desired cluster and click its ID.

4.  In the lower-right corner of the **Basic Information** page, choose **Configuration Update** \> **Upgrade**.

5.  Change the configuration of the cluster, such as node specifications, the storage type, and storage space per node.

    The **Current Config** section of the Upgrade/Downgrade page displays the current configuration of the cluster. You can use this as a reference during the upgrade.

    Follow the instructions on the Upgrade/Downgrade page to upgrade the configuration of your cluster based on your business requirements. For more information about the parameters on the page, see [Parameters on the buy page](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Parameters on the buy page.md). The following table describes only some of the parameters.

    **Note:** If the zone where your cluster resides has insufficient resources for a configuration upgrade, you can migrate the nodes in the zone to another before the upgrade. For more information, see [Migrate nodes in a zone](/intl.en-US/Elasticsearch Instances Management/Data migration/Migrate nodes in a zone.md).

    |Parameter|Description|
    |---------|-----------|
    |**Node Storage**|If **Disk Type** is set to **Cloud SSD**, you can increase the value of **Node Storage** for data nodes. The maximum storage space supported by a single node depends on the disk type of the node. You can check specific limits in the Elasticsearch console.|
    |**Dedicated Master Node**|You can purchase or upgrade dedicated master nodes. **Note:** If the specifications of your dedicated master nodes are 1 vCPU and 2 GiB of memory, you can upgrade these nodes on the Upgrade/Downgrade page. After the upgrade, the cluster is charged based on the new specifications. If your dedicated master nodes are free of charge, you are charged for these nodes after you upgrade them. |
    |**Warm Node**|You can purchase or upgrade warm nodes.|
    |**Client Node**|You can purchase or upgrade client nodes.|
    |**Kibana Node**|You can upgrade your Kibana node. **Note:** When you purchase a cluster, Alibaba Cloud provides a Kibana node for you free of charge. This Kibana node offers 1 vCPU and 2 GiB of memory. You can upgrade the Kibana node on the Upgrade/Downgrade page. |
    |**Forced Update**|If your cluster is in an abnormal state \(indicated by the color red or yellow\) and your services are severely affected, you must immediately upgrade the cluster configuration. In this case, we recommend that you select **Forced Update**. The system will perform a forced update regardless of the cluster status. The update requires only a short period of time. **Note:**

    -   After the forced update, the system restarts your cluster. During the restart, the services running on the cluster may be unstable.
    -   If you do not select **Forced Update**, the system uses the default mode to restart your cluster to make the changes take effect. For more information, see [Restart a cluster or node](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Restart a cluster or node.md). |

6.  Read and select Elasticsearch Terms of Service. Then, click **Buy Now**.

    After you complete the payment, the system restarts the cluster to make the changes take effect.


