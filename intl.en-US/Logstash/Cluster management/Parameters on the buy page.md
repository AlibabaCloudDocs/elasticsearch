# Parameters on the buy page

This topic describes the parameters on the Alibaba Cloud Logstash buy page. You can refer to this topic to purchase a Logstash cluster.

## Parameters

|Parameter|Description|
|---------|-----------|
|**Billing Method**|Logstash provides two billing methods: subscription and pay-as-you-go. You can select a billing method based on your requirements.-   Pay-as-you-go: We recommend that you purchase pay-as-you-go clusters for program development or functional tests.

You can choose **More** \> **Release** in the Actions column that corresponds to a cluster to release it.

-   Subscription: Discounts are offered for subscription clusters based on subscription duration. After you purchase a subscription cluster, you can request a refund within five days. Refunds are not provided five days after the date of purchase.

Manual renewal and auto-renewal are supported. For more information, see [Renew a cluster](). Subscription clusters cannot be manually released in the console. |
|**Region**|For information about supported regions, see [Regions and zones](#section_y1a_15h_ejw).|
|**Zone**|For more information about supported zones, see [Regions and zones](#section_y1a_15h_ejw).|
|**Network Type**|The value of this parameter can only be **VPC**.|
|**VPC**|Select a virtual private cloud \(VPC\) in the current region.**Note:** If you want to use an Elastic Compute Service \(ECS\) instance to access your Logstash cluster in a VPC, make sure that the ECS instance and Logstash cluster are connected to the same VPC. |
|**VSwitch**|After you select a VPC, all the available vSwitches in the current zone are displayed.|
|**Cluster Type**|Only **Standard Edition** is supported.|
|**Logstash Version**|7.4 and 6.7 are supported.|
|**Node Specifications**|Logstash provides different node specifications based on the following vCPU-to-memory ratios: 1:1, 1:2, 1:4, and 1:8. The actual specifications in the console shall prevail.-   1:1: 4 vCPUs and 4 GiB of memory, 8 vCPUs and 8 GiB of memory, 12 vCPUs and 12 GiB of memory, and 16 vCPUs and 16 GiB of memory
-   1:2: 2 vCPUs and 4 GiB of memory, 16 vCPUs and 32 GiB of memory, and 32 vCPUs and 64 GiB of memory
-   1:4: 2 vCPUs and 8 GiB of memory, 4 vCPUs and 16 GiB of memory, 8 vCPUs and 32 GiB of memory, and 16 vCPUs and 64 GiB of memory
-   1:8: 2 vCPUs and 16 GiB of memory, 4 vCPUs and 32 GiB of memory, and 8 vCPUs and 64 GiB of memory |
|**Disk Type**|Valid values:-   **Standard SSD**: This is the default value. A standard SSD provides a maximum of 2,048 GiB of storage space. Standard SSDs are suitable for online data analytics and searches that require high IOPS and quick responses.
-   **Ultra Disk**: An ultra disk provides a maximum of 5,120 GiB of storage space. Ultra disks are cost-effective and are suitable for scenarios such as logging and analyzing large volumes of data. |
|**Storage Space per Node**|The storage space of each node. It depends on the disk type. Unit: GiB.-   If the disk type is **Standard SSD**, the value of this parameter ranges from 20 to 2048.
-   If the disk type is **Ultra Disk**, the maximum value of this parameter is 5120.

**Note:** You can resize an ultra disk to a maximum of 2,048 GiB. Ultra disks with a storage space larger than 2,560 GiB cannot be resized because these disks are designed to run in disk arrays or RAID 0. |
|**Nodes**|The number of nodes that you want to purchase. Valid values: 1 to 20.|
|**Duration**|This parameter is displayed only when **Billing Method** is set to **Subscription**. The default value of this parameter is 1 Month. Valid values: 1 Month, 2 Months, 3 Months, 4 Months, 5 Months, 6 Months, 7 Months, 8 Months, 9 Months, 1 Year, 2 Years, and 3 Years.|

## Regions and zones

|Country/District|Region|Zone|
|----------------|------|----|
|China|China \(Beijing\)|Zone C, Zone D, Zone E, Zone F, Zone G, Zone H, and Zone J|
|China \(Hangzhou\)|Zone E, Zone F, Zone G, Zone H, Zone I, and Zone J|
|China \(Qingdao\)|Zone B and Zone C|
|China \(Shanghai\)|Zone B, Zone D, Zone E, Zone F, and Zone G|
|China \(Shenzhen\)|Zone A, Zone B, Zone C, Zone D, Zone E, and Zone F|
|China \(Zhangjiakou\)|Zone A, Zone B, and Zone C|
|China \(Hong Kong\)|Zone B, Zone C, and Zone D|
|Asia Pacific|Singapore \(Singapore\)|Zone A, Zone B, and Zone C|
|Australia \(Sydney\)|Zone A and Zone B|
|Malaysia \(Kuala Lumpur\)|Zone A and Zone B|
|Indonesia \(Jakarta\)|Zone A and Zone B|
|Japan \(Tokyo\)|Zone A and Zone B|
|Europe & Americas|US \(Virginia\)|Zone A and Zone B|
|US \(Silicon Valley\)|Zone A and Zone B|
|Germany \(Frankfurt\)|Zone A and Zone B|
|UK \(London\)|Zone A and Zone B|
|Middle East and India|India \(Mumbai\)|Zone A and Zone B|

