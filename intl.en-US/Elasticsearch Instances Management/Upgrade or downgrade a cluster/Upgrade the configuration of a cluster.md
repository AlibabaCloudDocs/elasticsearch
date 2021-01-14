---
keyword: upgrade the configuration of an Elasticsearch cluster
---

# Upgrade the configuration of a cluster

As your business develops, you may have higher requirements for the configuration of your Elasticsearch cluster. If the current configuration of your Elasticsearch cluster cannot meet your business requirements, you can upgrade its configuration. This topic describes how to upgrade the configuration of an Elasticsearch cluster and the related precautions.

The specifications and storage capacity of your cluster are evaluated. For more information, see [Evaluate specifications and storage capacity](/intl.en-US/Elasticsearch Instances Management/Quick Start/Preparations/Evaluate specifications and storage capacity.md).

## Precautions

-   Specification upgrades
    -   You can upgrade the configuration for only one type of node during each upgrade. The node types include data nodes, warm nodes, client nodes, dedicated master nodes, and Kibana nodes. If your Elasticsearch cluster is an Advanced Edition cluster, you can purchase elastic nodes for the cluster during the upgrade.
    -   You cannot change the disk types of nodes when you upgrade the configuration of your Elasticsearch cluster. You can only increase the storage space of nodes.
    -   You cannot perform operations such as reducing disk space or downgrading nodes to downgrade the configuration of your Elasticsearch cluster.

        **Note:** You can remove data nodes from your cluster to implement the downgrade. For more information about how to remove data nodes, see [Scale in an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Scale in an Elasticsearch cluster.md).

-   Impact on services
    -   After you upgrade the configuration of your Elasticsearch cluster, the system restarts the cluster to make the changes take effect. The time required for the restart depends on the specifications, data structure, and data volume of the cluster. In most cases, the restart requires a few hours. For more information, see [Restart a cluster or node](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Restart a cluster or node.md).

        **Note:** In most cases, if the load of a cluster is not high and indexes have replica shards, the cluster can still provide services during a restart. In some cases, however, access timeouts may occur during a restart. For example, a number of clusters are forced to restart at the same time, the cluster is heavily loaded and is not accessible, indexes do not have replica shards, or large amounts of data is written and queried. In these cases, we recommend that you design a retry mechanism on the client before you restart the cluster.

    -   If your Elasticsearch cluster is abnormal \(indicated by the color yellow or red\), you must select **Forced Update**. This may affect services.
-   Version upgrades

    You cannot upgrade the version of your Elasticsearch cluster when you upgrade the configuration of the cluster. For more information about how to upgrade the version of a cluster, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade/Upgrade the version of a cluster.md).

    **Note:** If you perform a version upgrade during a configuration upgrade, the system displays the "UpgradeVersionMustFromConsole" error message.

-   Changes in billing

    After you submit a configuration upgrade order, your Elasticsearch cluster is charged based on the new configuration.

    **Note:** During a configuration upgrade, you can check the price of your order on the **Upgrade/Downgrade** page in real time.


## Procedure

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the lower-right corner of the **Basic Information** page, choose **Configuration Update** \> **Upgrade**.

5.  On the Upgrade/Downgrade page, change the configuration of the cluster based on the following instructions:

    The **Current Config** section of the Upgrade/Downgrade page shows the current configuration of the cluster. You can use this as a reference during the upgrade.

    Follow the instructions on the Upgrade/Downgrade page to upgrade the configuration of your cluster based on your business requirements. For more information about the parameters on the page, see [Parameters on the buy page](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Parameters on the buy page.md). The following table describes only some of the parameters.

    **Note:** If the zone where your cluster resides has insufficient resources for a configuration upgrade, you can migrate the nodes in the zone to another before the upgrade. For more information, see [Migrate nodes in a zone](/intl.en-US/Elasticsearch Instances Management/Data migration/Migrate nodes in a zone.md).

    |Parameter|Description|
    |---------|-----------|
    |**Node Storage**|If **Disk Type** is set to **Cloud SSD**, you can increase the value of **Node Storage** for data nodes. The maximum storage space supported by a single node depends on the disk type of the node. You can check specific limits in the Elasticsearch console.|
    |**Dedicated Master Node**|You can purchase or upgrade dedicated master nodes.**Note:** If the specifications of your dedicated master nodes are 1 vCPU and 2 GiB of memory, you can upgrade these nodes on the **Upgrade/Downgrade** page. After the upgrade, the cluster is charged based on the new specifications. If your dedicated master nodes are free of charge, you are charged for these nodes after you upgrade them. |
    |**Warm Node**|You can purchase or upgrade warm nodes.|
    |**Client Node**|You can purchase or upgrade client nodes.|
    |**Kibana Node**|You can upgrade your Kibana node.**Note:** When you purchase a cluster, Alibaba Cloud provides a Kibana node for you free of charge. This Kibana node offers 1 vCPU and 2 GiB of memory. You can upgrade the Kibana node on the Upgrade/Downgrade page. |
    |**Forced Update**|If your Elasticsearch cluster is abnormal \(indicated by the color red or yellow\) and your services are severely affected, you must immediately upgrade the cluster configuration. In this case, we recommend that you select **Forced Update**. The system will perform a forced update regardless of the cluster status. The update requires only a short period.**Note:**

    -   After a forced update, the system restarts your cluster. During the restart, the services running on the cluster may be unstable.
    -   If you do not select **Forced Update**, the system uses the default mode to restart your cluster to make the changes take effect. For more information, see [Restart a cluster or node](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Restart a cluster or node.md). |

6.  Read and select Elasticsearch \(Subscription\) Terms of Service. Then, click **Buy Now**.

    After you complete the payment, the system restarts the cluster to make the changes take effect.


