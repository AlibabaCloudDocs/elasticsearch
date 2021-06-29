---
keyword: [FAQ about Elasticsearch access control, no VPC available for an Elasticsearch cluster to purchase, delete Elasticsearch clusters created by a temporary user]
---

# FAQ about access control

This topic provides answers to some frequently asked questions about the access control of Alibaba Cloud Elasticsearch clusters.

-   [When I use a RAM user to purchase an Elasticsearch cluster, no VPCs are available on the buy page. Why?](#section_xaw_ax9_ni0)
-   [If a temporary user is deleted, will Elasticsearch clusters or data that is created by the user be deleted?](#section_zi7_l2m_71k)
-   [When I use Elasticsearch, the error message "The specified RAM user is not authorized. Check the permission of the RAM user and try again." is displayed. What do I do?](#section_71e_o7x_dr2)
-   [How do I create a user that has read-only permissions on resources, such as indexes, of an Elasticsearch cluster?](#section_7bw_luq_ljd)
-   [When I use a user to which the required role is assigned to log on to the Kibana console, the console displays no indexes. Only the elastic account can be used to view indexes. What do I do?](#section_pl3_pa3_8qt)

## When I use a RAM user to purchase an Elasticsearch cluster, no VPCs are available on the buy page. Why?

Check whether the Resource Access Management \(RAM\) user has the permissions to obtain the list of virtual private clouds \(VPCs\). For more information, see [View the basic information of a RAM user](/intl.en-US/RAM User Management/Basic operations/View the basic information of a RAM user.md). If the RAM user does not have the required permissions, grant the permissions to the RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM/Grant permissions to a RAM user.md).

## If a temporary user is deleted, will Elasticsearch clusters or data that is created by the user be deleted?

If a temporary user is deleted, the Elasticsearch clusters that are created by this user will not be deleted. In addition, the changes made by this user to the Elasticsearch clusters will not be restored. Operations performed by a temporary user are equivalent to those performed by an Alibaba Cloud account.

## When I use Elasticsearch, the error message "The specified RAM user is not authorized. Check the permission of the RAM user and try again." is displayed. What do I do?

Grant one of the following permissions to the RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM/Grant permissions to a RAM user.md).

-   `AliyunElasticsearchReadOnlyAccess`: the read-only permissions on Elasticsearch or Logstash clusters. This policy can be attached to read-only users.
-   `AliyunElasticsearchFullAccess`: the management permissions on Elasticsearch or Logstash clusters. This policy can be attached to administrators.

## How do I create a user that has read-only permissions on resources, such as indexes, of an Elasticsearch cluster?

Create a role that has such permissions in the Kibana console. Then, assign the role to a user. For more information, see [Grant permissions on indexes](/intl.en-US/RAM/Manage Kibana role/Create a role.md).

## When I use a user to which the required role is assigned to log on to the Kibana console, the console displays no indexes. Only the elastic account can be used to view indexes. What do I do?

When you [create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md), grant the kibana\_system permission to the user.

![Grant the kibana_system permission](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8724309951/p103233.png)

