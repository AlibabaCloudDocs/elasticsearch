# Create a VPC and a vSwitch

Alibaba Cloud Elasticsearch clusters can be deployed only in virtual private clouds \(VPCs\). Before you purchase an Alibaba Cloud Elasticsearch cluster, you must create a VPC and a vSwitch.

For more information about how to create a VPC and a vSwitch, see [Create an IPv4 VPC](/intl.en-US/Quick Start/Create an IPv4 VPC.md).

During creation, take note of the following items:

-   If you want to use an Elastic Compute Service \(ECS\) instance to access your Elasticsearch cluster, the ECS instance must reside in the same region as the Elasticsearch cluster. Additionally, the following requirements must be met:
    -   If the ECS instance is deployed in a VPC, the Elasticsearch cluster must also be deployed in the VPC.
    -   If the ECS instance and Elasticsearch cluster are deployed in the same VPC and region but different zones, create a vSwitch in each of the zones. This ensures that the network is accessible between the zones.
-   The **VSwitch** drop-down list displays only the available VSwitches in a specific VPC that reside in the same zone as your Elasticsearch cluster.
    -   If no VSwitches are available in the zone, create a VSwitch in the zone. For more information, see [Create a VSwitch](/intl.en-US/Quick Start/Create an IPv4 VPC.md).
    -   At least 50 VSwitch IP addresses are available. Otherwise, the system displays the **Don't have enough private IPs in this switch** message.

