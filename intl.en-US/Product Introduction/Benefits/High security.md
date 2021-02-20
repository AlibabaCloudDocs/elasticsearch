# High security

Alibaba Cloud Elasticsearch clusters are deployed in logically isolated virtual private clouds \(VPCs\). In addition, access control, authentication and authorization, encryption, and the advanced security features provided by X-Pack are used for the clusters. All the preceding features ensure the high security of Alibaba Cloud Elasticsearch clusters. This topic describes the features.

## Background information

Open source software is often the first target of attacks. MongoDB ransomware attacks are an example. Elasticsearch has also become the target of attacks. Attackers may attack self-managed Elasticsearch clusters that do not have professional security protection, delete important data, or interfere with business systems.

Alibaba Cloud Security Center released a warning about the security risks associated with Elasticsearch and provided an array of security hardening strategies and solutions. Alibaba Cloud Elasticsearch provides solutions that are more reliable and professional for data and service security than those provided by open source Elasticsearch.

## Security features

Alibaba Cloud released the fully managed Elasticsearch service in November 2017. Alibaba Cloud Elasticsearch provides security protection features for you to safeguard your clusters.

The following table compares the security protection of an Alibaba Cloud Elasticsearch cluster with that of a self-managed Elasticsearch cluster.

|Category|Built-in security feature of an Alibaba Cloud Elasticsearch cluster|Security protection of a self-managed Elasticsearch cluster|
|--------|-------------------------------------------------------------------|-----------------------------------------------------------|
|[Access control](#section_ygw_klk_zgb)|-   Clusters are deployed in VPCs. This way, the clusters can be isolated at the data link layer.
-   Both Elasticsearch and Kibana support whitelists for access control. You can specify IPv4 addresses, IPv6 addresses, and Classless Inter-Domain Routing \(CIDR\) blocks in whitelists. By default, no IP address is allowed to access the public endpoint of a cluster. If you want to allow access requests, you must configure a whitelist. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).
-   Users are not allowed to log on to the node servers that are contained in a cluster.
-   Users can use only ports 9200 and 9300 to access the public and internal endpoints of clusters.

|-   Purchase cloud security products, such as security groups or Cloud Firewall, to manage and quarantine source IP addresses.
-   Disable port 9200 unless you plan to use it.
-   Bind source IP addresses.
-   Change the default port. |
|[Authentication and authorization](#section_p5f_msb_vel)|-   Cluster-level permission policies in Resource Access Management \(RAM\), such as the ReadOnlyAccess policy that grants read-only permissions and the FullAccess policy that grants administrator permissions.
-   RAM-based access control, such as the permissions on clusters, accounts, and GET, POST, and PUT commands.
-   Role-based access control \(RBAC\) provided by X-Pack. Access control policies can be specific to data fields.
-   Single sign-on \(SSO\) based on X-Pack. Active Directory, LDAP, and Elasticsearch-native Realm are supported for identity verification.

|Install third-party security plug-ins, such as Search Guard and Shield.|
|Encryption|-   HTTPS is supported.
-   Encryption at rest is provided based on Key Management Service \(KMS\).
-   X-Pack is integrated to support data transmission encryption by using SSL or TLS.

|-   Use storage media that support encryption at rest.
-   Disable HTTP in YML configuration files. |
|Monitoring and auditing|-   Operations log auditing based on X-Pack.
-   Cloud Monitor-based cluster monitoring with multiple metrics, such as cluster workload.

|Use third-party tools to audit logs and monitor services.|
|Disaster recovery|-   Snapshots are automatically created at a scheduled time.
-   A cluster can be deployed across zones in one city to implement disaster recovery.

|-   Purchase file systems to periodically back up data.
-   Use multiple clusters to implement disaster recovery. |

## Access control

Alibaba Cloud Elasticsearch uses the following methods to control access:

-   Access over VPCs

    You can use the internal endpoint of an Alibaba Cloud Elasticsearch cluster to access the cluster over a VPC. If you require a secure environment where your applications can access your Alibaba Cloud Elasticsearch cluster, you can purchase an Alibaba Cloud Elastic Compute Service \(ECS\) instance in the same zone, region, and VPC as the Elasticsearch cluster. Then, deploy the applications on the ECS instance and use the ECS instance to access the internal endpoint of the Elasticsearch cluster.

    **Note:** A VPC is a private network in the cloud and is isolated from the Internet. It provides secure access for your applications.

-   Whitelist-based access control

    If you want to use the internal endpoint of an Alibaba Cloud Elasticsearch cluster to access the cluster, configure a whitelist for the cluster to control access. Only clients whose IP addresses are in the whitelist can be used to access the cluster. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).

    If you want to use the public endpoint of an Alibaba Cloud Elasticsearch cluster to access the cluster, configure a whitelist for the cluster to control access. Only clients whose IP addresses are in the whitelist can be used to access the cluster. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).


## Authentication and authorization

-   RAM-based access control

    The Alibaba Cloud Elasticsearch console supports RAM users. You can use RAM users to isolate resources. A RAM user can view and manage only Alibaba Cloud Elasticsearch clusters on which the user has permissions. For more information, see [Policy check rules](/intl.en-US/Policy Management/Policy language/Policy check rules.md).

-   RBAC provided by X-Pack

    Alibaba Cloud Elasticsearch provides the X-Pack plug-in, which is a commercial extension of Elasticsearch. The plug-in is an easy-to-install bundle that provides security, alerting, monitoring, graphing, and reporting capabilities. The plug-in is integrated into Kibana to provide more capabilities, such as authentication and authorization, RBAC, real-time monitoring, visual reporting, and machine learning. RBAC can be specific to indexes. For more information, see [Use the Elasticsearch X-Pack RBAC mechanism to implement access control](/intl.en-US/Best Practices/Elasticsearch applications/Index management/Use the Elasticsearch X-Pack RBAC mechanism to implement access control.md) and [Security APIs in the open source Elasticsearch documentation](https://www.elastic.co/guide/en/elasticsearch/reference/5.5/security-api.html).


