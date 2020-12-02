---
keyword: [Logstash access to the Internet, access to Logstash over the Internet]
---

# Configure a NAT gateway for data transmission over the Internet

This topic describes how to configure a Network Address Translation \(NAT\) gateway to connect Alibaba Cloud Logstash in a Virtual Private Cloud \(VPC\) to the Internet.

You have completed the following operations:

-   A VPC and a VSwitch are created.

    For more information, see [Create an IPv4 VPC network](/intl.en-US/Quick Start/Create an IPv4 VPC network.md).

-   An Alibaba Cloud Logstash cluster is created.

    For more information, see [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).


1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Networks and Security**.

5.  In the **Network Settings** section, click **Configure NAT Gateway**.

    For more information about the descriptions and configurations of NAT gateways, see [NAT Gateway](). Destination Network Address Translation \(DNAT\) entries allow services on the Internet to send data to Logstash. Source Network Address Translation \(SNAT\) entries allow Logstash to access the Internet.

6.  On the NAT Gateways page, click Create NAT Gateway.

    When you create a NAT gateway, select the region and VPC that are the same as those of Alibaba Cloud Logstash. For more information, see [Create a NAT gateway](/intl.en-US/User Guide/NAT Gateway Instance/Create a NAT gateway.md).

7.  Associate an Elastic IP address with the NAT gateway.

    1.  On the NAT Gateways page, find the target NAT gateway and choose **![More](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8592986061/p98649.png)** \> **Bind Elastic IP Address** in the **Actions** column.

    2.  If Elastic IP addresses are available, click **Select from EIP List**

        If no Elastic IP addresses are available, click **Allocate and Bind EIP to NAT Gateway**. Complete configurations as prompted.

    3.  Select the target Elastic IP address and click **OK**.

        **Note:** A NAT gateway can be associated with up to 20 Elastic IP addresses. Among these Elastic IP addresses, a maximum of 10 pay-as-you-go Elastic IP addresses can be associated and each of the pay-as-you-go Elastic IP addresses supports a peak throughput of 200 Mbit/s. You can submit a ticket to increase the number of Elastic IP addresses that can be used.

8.  Create a DNAT entry.

    1.  On the NAT Gateways page, find the NAT gateway and click **Configure DNAT** in the **Actions** column.

    2.  On the **DNAT Table** page, click **Create DNAT Entry**.

    3.  In the **Create DNAT Entry** pane, set parameters.

        ![Create a DNAT entry](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8592986061/p67462.png)

        |Parameter|Description|
        |---------|-----------|
        |**Public IP Address**|Select an available public IP address. **Note:** If a public IP address is already used in a SNAT entry, it cannot be used to create a DNAT entry. |
        |**Private IP Address**|Click **Manually Input** and enter the IP addresses of Logstash nodes. You can obtain these IP addresses on the basic information page of the Logstash cluster.|
        |**Port Settings**|Choose a DNAT mapping method.         -   **All**: This method uses IP mapping and associates an Elastic IP address with the Logstash cluster. All requests destined for the public IP address are forwarded to the Logstash cluster.
        -   **Specific Port**: This method uses port mapping. The NAT gateway forwards the requests from the specified protocol and port to the specified port of the Logstash cluster.

If you choose **Specific Port**, you must specify **Public Port** \(the external port for port forwarding\), **Private Port** \(the internal port for port forwarding\), and **IP Protocol** \(the type of the protocol for port forwarding\). |

    4.  Click **OK**.

9.  Create a SNAT entry.

    1.  Return to the NAT Gateways page. Find the NAT gateway and click **Configure SNAT** in the **Actions** column.

    2.  On the **SNAT Table** page, click **Create SNAT Entry**.

    3.  In the **Create SNAT Entry** pane, click **VSwitch Granularity** and set parameters.

        ![Create a SNAT entry](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8592986061/p67464.png)

        |Parameter|Description|
        |---------|-----------|
        |**VSwitch**|Select a VSwitch from the VPC where the Logstash cluster resides. All ECS instances under the specified VSwitch can access the Internet by using the SNAT feature.|
        |**Public IP Address**|Select the public IP address that is used to access the Internet. You can select multiple public IP addresses to build a SNAT IP address pool. If you select multiple public IP addresses to build a SNAT IP address pool, make sure that each public IP address is added to the same EIP Bandwidth Plan instance. For more information, see [Associate an EIP with an EIP bandwidth plan](/intl.en-US/User Guide/Manage Pay-As-You-Go-billed EIPs/Associate an EIP with an EIP bandwidth plan.md). |

        For more information about the parameters, see [t395012.md\#](/intl.en-US/User Guide/SNAT/Create a SNAT entry.md).

    4.  Click **OK**.


