---
keyword: upgrade the configuration of an Elasticsearch cluster
---

# Upgrade the configuration of a cluster

As your business develops, you may have higher requirements for the configuration of your Elasticsearch cluster. If the current configuration of your Elasticsearch cluster cannot meet your business needs, you can upgrade its configuration. This topic describes how to upgrade the configuration of an Elasticsearch cluster and the related precautions.

The specifications and storage capacity of your cluster are evaluated. For more information, see [Evaluate specifications and storage capacity](/intl.en-US/Quick Start/Preparations/Evaluate specifications and storage capacity.md).

## Precautions

-   Specification upgrade
    -   For each upgrade, you can upgrade the configuration for only one type of node. The node types include data nodes, warm nodes, client nodes, dedicated master nodes, and Kibana nodes.
    -   You cannot change the disk types of nodes when you upgrade the configuration of your Elasticsearch cluster. You can only increase the storage space of nodes.
    -   You cannot reduce disk space or downgrade nodes to downgrade the configuration of your Elasticsearch cluster.

        **Note:** You can remove data nodes from your cluster to implement the downgrade. For more information about how to remove data nodes and the related limits, see [Scale in an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Scale in an Elasticsearch cluster.md).

-   Impact on services
    -   If your Elasticsearch cluster is abnormal \(indicated by the color yellow or red\), you must select **Forced Update**. This may affect services.
    -   In most cases, the system restarts your Elasticsearch cluster after a configuration upgrade to make the changes take effect. However, if your cluster contains dedicated master nodes and you only change the **number of nodes**, the system does not restart the cluster.
-   Version upgrade

    You can only upgrade your Elasticsearch cluster from V6.3.2 to V6.7.0. A version upgrade cannot be performed during a configuration upgrade. For more information, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the version of a cluster.md).

    **Note:** If you perform a version upgrade during a configuration upgrade, the system displays the "UpgradeVersionMustFromConsole" error message.

-   Changes in billing

    After you submit a configuration upgrade order, your Elasticsearch cluster is charged based on the new configuration.

    **Note:** During a configuration upgrade, you can check the price of your order on the **Update** page in real time.


## Procedure

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the lower-right corner of the **Basic Information** page, choose **Configuration Update** \> **Upgrade**.

5.  On the Update page, change the configuration of the cluster based on the following instructions:

    **Note:** The **Current Config** section on the Update page shows the current configuration of the cluster. You can use this as a reference during the upgrade.

    Follow the instructions on the Update page to upgrade the configuration of your cluster based on your business requirements. For more information about the parameters on the Update page, see [Parameters on the buy page](/intl.en-US/Quick Start/Step 1: Create a cluster/Parameters on the buy page.md). The following table describes only some of the parameters.

    **Note:** If the zone where your cluster resides has insufficient resources for a configuration upgrade, you can migrate the nodes in the zone to another before the upgrade. For more information, see [Migrate nodes in a zone](/intl.en-US/Elasticsearch Instances Management/Data migration/Migrate nodes in a zone.md).

    |Parameter|Description|
    |---------|-----------|
    |**Node Storage**|If **Category** is set to **Cloud Disk**, you can increase the value of **Node Storage** for **data nodes**. The maximum storage space supported by a single node depends on the **disk type** of the node. You can check specific limits in the Elasticsearch console.|
    |**Forced Update**|If your Elasticsearch cluster is abnormal \(indicated by the color red or yellow\) and your services are severely affected, you must immediately upgrade the cluster configuration. In this case, we recommend that you select **Forced Update**. The system will perform a forced update regardless of the cluster status. The update requires only a short period. **Note:**

    -   After a **forced update**, the system restarts your cluster. During the restart, the services running on the cluster may be unstable.
    -   If you do not select **Forced Update**, the system uses the **default mode** to restart your cluster to make the changes take effect. For more information, see [Restart a cluster or node](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Restart a cluster or node.md).
    -   |
    |**Dedicated Master Node**|You can purchase or upgrade dedicated master nodes. **Note:** If the specifications of your dedicated master nodes are **1 vCPU and 2 GiB of memory**, you can upgrade these nodes on the **Update** page. After the upgrade, the cluster is charged based on the new specifications. If your dedicated master nodes are free of charge, you are charged for these nodes after you upgrade them. |
    |**Client Node**|You can purchase or upgrade client nodes.|
    |**Warm Node**|You can purchase or upgrade warm nodes.|
    |**Kibana Node**|You can upgrade your Kibana node. **Note:** When you purchase a cluster, Alibaba Cloud provides a Kibana node for you free of charge. This Kibana node offers 1 vCPU and 2 GiB of memory. You can upgrade the Kibana node on the Update page. |

6.  Read and agree to the terms of cluster configuration upgrades. Then, click **Buy Now** and complete the payment as prompted.

    After you complete the payment, the system restarts the cluster to make the changes take effect.


