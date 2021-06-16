# Parameters on the buy page

When you purchase an Alibaba Cloud Elasticsearch cluster, follow the instructions in this topic to configure the parameters on the buy page.

## Billing methods

Elasticsearch provides two billing methods: **subscription** and **pay-as-you-go**. You can select a billing method based on your requirements.

-   **Pay-as-you-go**: We recommend that you purchase **pay-as-you-go** Elasticsearch clusters for program development or functional tests.

    You can log on to the Elasticsearch console, click **More** in the Actions column that corresponds to an Elasticsearch cluster, and then select **Release** to manually release the cluster.

-   **Subscription**: Discounts are offered for subscription Elasticsearch clusters based on subscription duration. Refunds are not provided after the date of purchase.

    Manual renewal and auto-renewal are supported. For more information, see [Enable auto renewal]() and [t1880497.md\#](/intl.en-US/Pricing/Manually renew an Elasticsearch cluster.md). Subscription Elasticsearch clusters cannot be manually released in the console.


## Basic settings

|Parameter|Description|
|---------|-----------|
|**Instance Type**|The value of this parameter can be only **X-Pack Version**.|
|**Elasticsearch Version**|Valid values: 7.10, 7.7, 6.8, 6.7, 6.3, 5.6, and 5.5. **Note:**

-   After May 2021, V7.4 is no longer provided. Existing V7.4 clusters are not affected. We recommend that you purchase V7.10 clusters.
-   We recommend that you select a later version. You may encounter differences in performance optimization and bug fixes among different versions. For more information about these differences, see the [release notes for open source Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/es-release-notes.html) of each version. |
|**Region**|For more information, see [Regions and zones](#section_9b2_83l_qmy).|
|**Zone**|For more information, see [Regions and zones](#section_9b2_83l_qmy).|
|**Number of Zones**|-   **1-AZ**: This is the default deployment method. It is used to handle non-critical workloads.
-   **2-AZ**: This deployment method implements cross-zone disaster recovery. It is used to handle production workloads.
-   **3-AZ**: This deployment method implements high availability. It is used to handle production workloads that require high service availability.

**Note:**

-   You can deploy an Elasticsearch cluster across three zones only in the China \(Hangzhou\), China \(Beijing\), China \(Shanghai\), or China \(Shenzhen\) region.
-   When you deploy an Elasticsearch cluster across zones, you do not need to specify each zone. The system automatically selects the zones.
-   For more information about the precautions for deploying and using multi-zone Elasticsearch clusters, see [Deploy and use a multi-zone Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Deploy and use a multi-zone Elasticsearch cluster.md). |
|**Network Type**|The value of this parameter can be only **VPC**.|
|**VPC**|Select a virtual private cloud \(VPC\) in the current region. **Note:** If you want to use an Elastic Compute Service \(ECS\) instance to access your Elasticsearch cluster in a VPC, make sure that the ECS instance and Elasticsearch cluster reside in the same VPC. |
|**VSwitch**|After you specify a VPC, all the available vSwitches in the selected zone are displayed.|

## Regions and zones

The following table lists the [regions and zones](#section_9b2_83l_qmy) where Elasticsearch is available.

|Country or District|Region|Zone|
|-------------------|------|----|
|China|China \(Hangzhou\)|Zone K, Zone I, Zone J, Zone H, Zone G, Zone F, and Zone E|
|China \(Beijing\)|Zone K, Zone J, Zone I, Zone H, Zone G, Zone F, Zone E, Zone D, and Zone C|
|China \(Shanghai\)|Zone L, Zone G, Zone F, Zone E, Zone D, and Zone B|
|China \(Shenzhen\)|Zone F, Zone E, Zone D, Zone C, Zone B, and Zone A|
|China \(Qingdao\)|Zone B and Zone C|
|China \(Zhangjiakou\)|Zone B and Zone A|
|China \(Hong Kong\)|Zone D, Zone C, and Zone B|
|Asia Pacific|Singapore \(Singapore\)|Zone C, Zone B, and Zone A|
|Malaysia \(Kuala Lumpur\)|Zone B and Zone A|
|Japan \(Tokyo\)|Zone B and Zone A|
|Australia \(Sydney\)|Zone B and Zone A|
|Indonesia \(Jakarta\)|Zone B and Zone A|
|Europe & Americas|US \(Virginia\)|Zone B and Zone A|
|US \(Silicon Valley\)|Zone B and Zone A|
|Germany \(Frankfurt\)|Zone B and Zone A|
|Middle East & India|India \(Mumbai\)|Zone B and Zone A|

## Node settings

|Parameter|Description|
|---------|-----------|
|**Data Node Type**|[Data nodes](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/modules-node.html#data-node) store index data. You can use data nodes to add, delete, search for, modify, or aggregate data in documents. Data nodes have high CPU, memory, and I/O requirements. When you optimize the performance of an Elasticsearch cluster, you must monitor the statuses of the data nodes in the cluster. If the resources of the cluster are insufficient, we recommend that you add data nodes to the cluster. Data nodes support specifications such as 2C 4GB and 2C 8GB.

**Note:**

-   Since May 2021, Alibaba Cloud Elasticsearch no longer provides data nodes with the 1C 2GB or 2C 2GB specifications due to their impact on performance stability. Existing data nodes with such specifications are not affected.
-   The 1C 2GB specifications are designed for testing purposes. Do not use clusters whose data nodes are of such specifications for production purposes. The service-level agreement \(SLA\) does not apply to these clusters. Therefore, we recommend that you upgrade your data nodes with such specifications. |
|**Data Nodes**|The number of data nodes that you want to purchase. The default value of this parameter is 3. Valid values: 2 to 50. **Note:** You must purchase a minimum of two data nodes. However, a cluster that contains only two data nodes has a greater risk of split-brain. Therefore, exercise caution when you set this parameter. |
|**Dedicated Master Node**|You can use [dedicated master nodes](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/modules-node.html?spm=a2c4g.11186623.2.32.10461b96Q7gfNV#master-node) to perform operations on clusters. You can create or delete indexes, track nodes, and allocate shards. The stability of dedicated master nodes is important to the health of clusters. By default, every node in a cluster may be selected as a dedicated master node. Operations, such as data indexing, searches, and queries, require a large number of CPU, memory, and I/O resources. To ensure the stability of a cluster, we recommend that you purchase dedicated master nodes to separate the dedicated master nodes from data nodes. The default value of this parameter is No for a cluster deployed only in one zone and is Yes for a cluster deployed across zones. On the buy page or [configuration upgrade](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md) page, click Yes next to Dedicated Master Node to purchase dedicated master nodes. You can also upgrade purchased dedicated master nodes on the configuration upgrade page. Your cluster is then charged based on the new specifications. For more information about the prices of the specifications, see [Pricing](https://www.alibabacloud.com/product/elasticsearch).

**Note:**

-   To improve the stability of your services, we recommend that you purchase dedicated master nodes.
-   You cannot release the dedicated master nodes that you have purchased.
-   If you have purchased 10 or more data nodes, the default value of this parameter is No. You must manually purchase dedicated master nodes.
-   If the dedicated master nodes in your cluster are free of charge, you are charged for these nodes after you [upgrade the configuration of the cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).
-   If dedicated master nodes are purchased and the value of the Dedicated Master Node parameter on the configuration upgrade page is Yes, the specifications of the dedicated master nodes are 1C 2GB.

After you set this parameter to Yes, you can set the following parameters:

-   **Dedicated Master Nodes**

The value of this parameter can be only 3.

-   **Dedicated Master Node Type**

The default value of this parameter is 2C 8GB, which is the minimum specifications for a dedicated master node. You can set the parameter based on your requirements.

**Note:** You cannot downgrade dedicated master nodes.

-   **Dedicated Master Node Disk Type**

The value of this parameter can be only Cloud SSD.

-   **Dedicated Master Node Storage Space**

The value of this parameter can be only 20G. |
|**Client Node**|You can purchase [client nodes](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/modules-node.html?spm=a2c4g.11186623.2.36.10461b96Q7gfNV#coordinating-only-node) to share the CPU overheads of data nodes. This further improves the computing performance and service stability of your Elasticsearch cluster. For CPU-intensive services, we recommend that you purchase client nodes. For example, if a large number of aggregation or query operations are performed, you can use client nodes to share overheads. For more information, see [Node types of open source Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/modules-node.html). The default value of this parameter is No. On the buy page or [configuration upgrade page](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md), click Yes next to Client Node to purchase client nodes. You can also upgrade purchased client nodes on the configuration upgrade page. Your cluster is then charged based on the new specifications. For more information about the prices of the specifications, see [Pricing](https://www.alibabacloud.com/product/elasticsearch).

**Note:** You cannot release the client nodes that you have purchased.

After you set this parameter to Yes, you can set the following parameters:

-   **Client Nodes**

The default value of this parameter is 2. Valid values: 2 to 25.

-   **Client Node Type**

The default value of this parameter is 2C 8GB. You can set the parameter based on your requirements.

**Note:** You cannot downgrade client nodes.

-   **Client Node Disk Type**

The value of this parameter can be only Efficient cloud disk.

-   **Client Node Storage Space**

The value of this parameter can be only 20G. |
|**Warm Node**|If your business includes both of the following index types, we recommend that you purchase warm nodes to implement the hot-warm architecture. This architecture improves the computing performance and service stability of your Elasticsearch cluster. For more information, see ["Hot-Warm" Architecture in Elasticsearch 5.x](https://www.elastic.co/cn/blog/hot-warm-architecture-in-elasticsearch-5-x?spm=a2c4g.11186623.2.34.10461b96Q7gfNV). -   Frequently queried or written indexes
-   Infrequently queried or written indexes, typically indexes of records

The default value of this parameter is No. On the buy page or [configuration upgrade](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md) page, click Yes next to Warm Node to purchase warm nodes. You can also upgrade purchased warm nodes on the configuration upgrade page. Your cluster is then charged based on the new specifications. For more information about the prices of the specifications, see [Pricing](https://www.alibabacloud.com/product/elasticsearch).

**Note:** You cannot release the warm nodes that you have purchased.

After you purchase nodes, the system adds `-Enode.attr.box_type` to their startup parameters.

-   Data nodes: `-Enode.attr.box_type=hot`
-   Warm nodes: `-Enode.attr.box_type=warm`

After you set this parameter to Yes, you can set the following parameters:

-   **Warm Nodes**

The default value of this parameter is 2. Valid values: 2 to 25.

-   **Warm Node Type**

The default value of this parameter is 2C 8GB. You can set the parameter based on your requirements.

**Note:** You cannot downgrade warm nodes.

-   **Warm Node Disk Type**

The value of this parameter can be only Efficient cloud disk.

-   **Warm Node Disk Encryption**

Disk encryption offers the maximum data security without requiring you to make additional changes to your business and applications. However, disk encryption may have a small impact on the performance of your Elasticsearch cluster. Disk encryption is free of charge. Reading data from or writing data to encrypted disks does not generate additional fees.

-   **Warm Node Storage Space**

The minimum value of this parameter is 500. Unit: GiB. You can set the parameter based on your requirements. |
|**Kibana Node**|The value of this parameter can be only Yes.|
|**Kibana Node Type**|Alibaba Cloud offers you a free Kibana node with the specifications of **1C 2GB**. You can choose to purchase a Kibana node with higher specifications.|
|**Username**|The username of the account that is used to access the Elasticsearch cluster and log on to the Kibana console. The default value of this parameter is elastic. **Note:** If you use the elastic account to access your Elasticsearch cluster and then reset the password of the account, it may require some time for the new password to take effect. During this period, you cannot use the elastic account to access the cluster. Therefore, we recommend that you do not use the elastic account to access an Elasticsearch cluster. You can log on to the Kibana console and create a user with the required role to access an Elasticsearch cluster. For more information, see [Create a role](/intl.en-US/RAM/Manage Kibana role/Create a role.md) and [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md). |
|**Password**|The password of the elastic account. You must set this parameter.|

## Storage settings

|Parameter|Description|
|---------|-----------|
|**Disk Type**|The disk type of the Elasticsearch cluster. Valid values: -   **Cloud SSD**: This is the default value. A standard SSD provides a maximum of 2,048 GiB of storage space. Standard SSDs are ideal for online data analysis and searches that require high IOPS and fast responses.
-   **Efficient cloud disk**: Ultra disks are cost-effective and are ideal for logging and analyzing large volumes of data.

**Note:** An ultra disk with a storage space greater than 2,560 GiB cannot be resized because the disk is designed to run in disk arrays or RAID 0. |
|**Disk Encryption**|Disk encryption offers the maximum data security without requiring you to make additional changes to your business and applications. However, disk encryption may have a small impact on the performance of your Elasticsearch cluster. Disk encryption is free of charge. Reading data from or writing data to encrypted disks does not generate additional fees. **Note:**

-   Only cloud disks can be encrypted.
-   You cannot enable disk encryption for purchased disks.
-   You cannot disable disk encryption for encrypted disks.
-   During a [cluster configuration upgrade](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md), you cannot change the disk encryption attribute for the disks that are purchased. However, you can enable disk encryption when you purchase warm nodes and cloud disks. |
|**Node Storage**|The storage space of each node. It depends on the disk type. Unit: GiB. -   If the disk type is **Cloud SSD**, the maximum value of this parameter is 2048.
-   If the disk type is **Efficient cloud disk**, the maximum value of this parameter is 5120.
    -   If the volume of the data that you want to store exceeds 2,048 GiB, you can set this parameter to 2560, 3072, 3584, 4096, 4608, or 5120.
    -   If the storage space of the disk for a purchased Elasticsearch cluster is less than 2,048 GiB, you can resize the disk to a maximum of 2,048 GiB. If the storage space of the disk is greater than 2,048 GiB, you cannot resize the disk. |

## Purchase plan

|Parameter|Description|
|---------|-----------|
|**Duration**|This parameter is available only for subscription clusters. The default value of this parameter is 1 Month. Valid values: 1 Month, 2 Months, 3 Months, 4 Months, 5 Months, 6 Months, 7 Months, 8 Months, 9 Months, 1 Year, 2 Years, and 3 Years.|
|**Auto-renewal**|Only subscription clusters support the auto-renewal feature. This feature is disabled by default. -   You can select **Auto-renewal** to enable this feature.
-   For purchased **subscription** Elasticsearch clusters, you can enable this feature in the [Billing Management console](https://renew.console.aliyun.com/center#/renew/elasticsearchpre). For more information, see [Enable auto renewal]().

**Note:**

    -   Monthly subscription: The auto-renewal cycle is one month.
    -   Yearly subscription: The auto-renewal cycle is one year. |

## Node types

The following table lists the node types supported by Alibaba Cloud Elasticsearch.

|Node type|Description|
|---------|-----------|
|[Data node](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/modules-node.html#data-node)|If dedicated master nodes are purchased, data nodes are used only as data nodes. If no dedicated master nodes are purchased, data nodes are also used as dedicated master nodes.|
|[Dedicated master node](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/modules-node.html#master-node)|Dedicated master nodes are used only as dedicated master nodes.|
|[Client node](https://www.elastic.co/guide/en/elasticsearch/reference/6.3/modules-node.html#coordinating-only-node)|Client nodes are used only as client nodes.|
|[Warm node](https://www.elastic.co/cn/blog/hot-warm-architecture-in-elasticsearch-5-x?spm=a2c4g.11186623.2.35.44ca610bMs12U2)|If no dedicated master nodes are purchased, warm nodes are used as both data nodes and dedicated master nodes. If dedicated master nodes are purchased, warm nodes are used only as data nodes.|

