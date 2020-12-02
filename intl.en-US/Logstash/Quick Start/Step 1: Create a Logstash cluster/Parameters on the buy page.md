# Parameters on the buy page

This topic describes the parameters on the buy page of an Alibaba Cloud Logstash cluster.

## Billing methods

Logstash provides two billing methods: **Subscription** and **Pay-As-You-Go**. You can select a billing method as required.

-   **Subscription**: Discounts are offered for subscription clusters based on subscription duration. After you purchase a subscription cluster, you can request a refund within five days. Refunds are not provided five days after the date of purchase.
    -   Subscription clusters support the auto-renewal feature. This feature is disabled by default. You can enable this feature in the console.
    -   Subscription clusters cannot be manually released in the console.
-   **Pay-As-You-Go**: We recommend that you purchase **pay-as-you-go** clusters for program development or functional tests. You can manually release a pay-as-you-go Logstash cluster in the console.

## Parameters

|Parameter|Description|
|---------|-----------|
|**Version**|Valid values: **6.7** and **7.4**.|
|**Region and Zone**|For more information, see [Regions and zones](#section_24a_luy_io8).|
|**Category**|For more information, see [Specifications](#section_2jb_rjr_5f8).|
|**Disk Type**|Valid values: -   **Cloud SSD**: This is the default value. A standard SSD provides a maximum of 2,048 GiB of storage space. Standard SSDs are suitable for online data analytics and search that require high IOPS and fast responses.
-   **Efficient cloud disk**: An ultra disk provides a maximum of 5,120 GiB of storage space. Ultra disks are cost-effective and are suitable for scenarios such as logging and analyzing large amounts of data. |
|**Node Storage**|The storage space of each node. It depends on the disk type. Unit: GiB. -   If the disk type is **Cloud SSD**, the value of this parameter ranges from 20 to 2048.
-   If the disk type is **Efficient cloud disk**, the maximum value of this parameter is 5120.

**Note:** You can resize an ultra disk to a maximum of 2,048 GiB. Ultra disks with the storage space larger than 2,560 GiB cannot be resized because these disks are designed to run in disk arrays or RAID 0. |
|**Nodes**|The number of nodes that you want to purchase. Valid values: 1 to 20.|
|**Network Type**|The value of this parameter can only be **VPC**.|
|**VPC**|Select a Virtual Private Cloud \(VPC\) in the current region. **Note:** If you want to use an Elastic Compute Service \(ECS\) instance to access your Logstash cluster in a VPC, make sure that the ECS instance and Logstash cluster are connected to the same VPC. |
|**VSwitch**|After you specify a VPC, all the available VSwitches in the current zone are displayed.|
|**Duration**|Only subscription clusters support this parameter. The default value of this parameter is 1 month. Valid values: 1 month, 2 month, 3 month, 4 month, 5 month, 6 month, 7 month, 8 month, 9 month, 1 yr, 2 yr, and 3 yr.|
|**Auto Renew**|Only subscription clusters support the auto-renewal feature. You can select Auto Renew to enable this feature. -   Monthly subscription: The auto-renewal cycle is one month.
-   Annual subscription: The auto-renewal cycle is one year. |

## Regions and zones

The following table lists the regions and zones where Logstash clusters are available.

|Country/District|Region|Zone|
|----------------|------|----|
|China|China \(Shenzhen\)|Zone A, Zone B, Zone C, Zone D, and Zone E|
|China \(Beijing\)|Zone C, Zone D, Zone E, Zone F, Zone G, and Zone H|
|China \(Qingdao\)|Zone B and Zone C|
|China \(Shanghai\)|Zone B, Zone D, Zone E, Zone F, and Zone G|
|China \(Hong Kong\)|Zone B and Zone C|
|China \(Zhangjiakou-Beijing Winter Olympics\)|Zone A and Zone B|
|China \(Hangzhou\)|Zone E, Zone F, Zone G, Zone H, and Zone I|
|Asia Pacific|Japan \(Tokyo\)|Zone A|
|Singapore|Zone A and Zone B|
|Australia \(Sydney\)|Zone A|
|Malaysia \(Kuala Lumpur\)|Zone A and Zone B|
|Indonesia \(Jakarta\)|Zone A|
|Europe & Americas|Germany \(Frankfurt\)|Zone A and Zone B|
|US \(Virginia\)|Zone A and Zone B|
|US \(Silicon Valley\)|Zone A and Zone B|
|Middle East & India|India \(Mumbai\)|Zone A|

## Specifications

Logstash provides different specifications based on vCPU-to-memory ratios. The following table lists the specifications. For more information about the prices of the specifications, see [Pricing]().

|vCPU-to-memory ratio|Specification|
|--------------------|-------------|
|1:1|4C 4GiB, 8C 8GiB, 12C 12GiB, and 16C 16GiB|
|1:2|2C 4GiB, 16C 32GiB, and 32C 64GiB|
|1:4|2C 8GiB, 4C 16GiB, 8C 32GiB, and 16C 64GiB|
|1:8|2C 16GiB, 4C 32GiB, and 8C 64GiB|

