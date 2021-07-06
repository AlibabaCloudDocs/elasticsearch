# FAQ about Alibaba Cloud Elasticsearch clusters

This topic provides answers to some frequently asked questions about Alibaba Cloud Elasticsearch clusters.

-   FAQ about the purchase, subscription cancellation, or release of clusters
    -   [When I purchase an Elasticsearch cluster, I performed an incorrect configuration. How do I modify the configuration after the cluster is created?](#section_pqi_rb3_djh)
    -   [What are the mappings between versions on the Elasticsearch buy page and specific Elasticsearch versions?](#section_ecn_32x_4n1)
    -   [When I purchase an Elasticsearch cluster, no VPCs are available. What do I do?](#section_8k3_wpr_32f)
    -   [When I purchase an Elasticsearch cluster, I select a VPC, but no vSwitches are available. What do I do?](#section_3ua_8iw_s0v)
    -   [After I cancel the subscription of or release an Elasticsearch cluster, I purchase another cluster. Does the endpoint of the new cluster remain the same as that of the original cluster?](#section_3el_7d8_y1n)
    -   [How do I release an Elasticsearch cluster?](#section_p92_cav_ju9)
    -   [When does the system release an Elasticsearch cluster after the cluster is suspended?](#buyb)
    -   [Can I purchase an Elasticsearch cluster that has only one node?](#buyc)
    -   [When I purchase an Elasticsearch cluster, resources of a specific category are sold out. What do I do?](#buyd)
-   FAQ about features
    -   [Can I upgrade or downgrade the version of an Elasticsearch cluster?](#section_l6j_4pw_luc)
    -   [Can I log on to an Elasticsearch cluster over SSH and modify the configuration of the cluster?](#section_66y_1yu_14l)
    -   [Is Logstash V6.7 compatible with Elasticsearch V6.3?](#section_jjo_687_oxh)
    -   [Can Elasticsearch be used as a data source of Quick BI?](#section_wzs_gwf_dt0)
    -   [Does Elasticsearch support scoring plug-ins?](#section_qwb_pxa_t7i)
    -   [Does Elasticsearch support Lightweight Directory Access Protocol \(LDAP\)?](#section_0b9_ew5_2bn)
    -   [Does Alibaba Cloud provide Elasticsearch SDK for Java?](#aa)
    -   [How do I view the kernel version of an Elasticsearch cluster?](#section_6ee_70y_8kh)
-   FAQ about cluster restart
    -   [How long is required to restart an Elasticsearch cluster or node?](#section_a97_rci_u7u)
    -   [Does the system restart an Elasticsearch cluster after I enable or disable the Public Network Access feature for the cluster?](#section_lsf_6pk_nd8)
-   FAQ about data read or write

    [The CPU utilization and loads of some nodes in an Elasticsearch cluster are normal, whereas other nodes are in the idle state. What do I do?](#section_zto_kls_xvc)

-   FAQ about cluster configurations and configuration modifications
    -   [How do I plan resources before I use Elasticsearch, such as cluster specifications, the number of shards, and the size of each shard?](#section_yus_2re_7ho)
    -   [How do I view the configuration of an Elasticsearch cluster?](#section_1hd_bc2_sjo)
    -   [Are services affected when I modify the configuration of an Elasticsearch cluster?](#section_mri_z28_4kc)
    -   [Can I change the cloud disk type of an Elasticsearch cluster?](#section_9ht_ho3_e90)
    -   [Can I convert other types of nodes in an Elasticsearch cluster to warm nodes?](#section_cx4_sm8_2jp)
    -   [Can I downgrade the specifications of an Elasticsearch cluster? If yes, how do I do?](#section_xp7_7ct_mjg)
    -   [How do I modify the configuration of an Elasticsearch cluster to ensure that services run as expected when a temporary business surge occurs?](#section_rdh_36n_v9k)
    -   [When I upgrade the configuration of an Elasticsearch cluster, the system displays the "UpgradeVersionMustFromConsole" error message. What do I do?](#ymlb)
    -   [How long is required to upgrade the version of an Elasticsearch cluster?](#ymlc)
    -   [Are services affected when I upgrade the version of an Elasticsearch cluster?](#ymld)
    -   [Can I change the JVM parameter settings of an Elasticsearch cluster?](#ymla)
    -   [Can I use the YML configuration file of an Elasticsearch cluster to configure the http.max\_content\_length and discovery.zen.ping\_timeout parameters?](#section_e54_c8p_2ei)
    -   [Can I switch the virtual private cloud \(VPC\) of an Elasticsearch cluster?](#section_sse_1yg_dng)
-   FAQ about plug-ins, tokens, and synonyms
    -   [How do I update dictionary content when I use the IK analysis plug-in?](#section_7du_5a2_uzc)
    -   [When I use the IK analysis plug-in, the system displays the "ik startOffset" error message. What do I do?](#section_msh_ad9_he4)
    -   [The IK dictionary files on my on-premises machine are lost. Can I retrieve them on the cluster management page?](#section_swe_lom_z23)
    -   [How do I apply updated IK dictionaries to existing data?](#section_ewc_qn2_fey)
    -   [Is a threshold specified for full garbage collection \(GC\)?](#section_4k3_xz1_0o2)
    -   [Can I remove plug-ins that are not used?](#ra)
    -   [Are the dictionaries provided by the IK analysis plug-in of Alibaba Cloud Elasticsearch the same as the dictionaries provided by the IK analysis plug-in of open source Elasticsearch?](#rb)
    -   [Can a custom plug-in access an external network, such as reading dictionary files on GitHub?](#rc)
    -   [Does a custom plug-in support the rolling update method?](#rd)
    -   [How do I configure the analysis-aliws plug-in? What is the format of the dictionary file for this plug-in?](#re)
    -   [What are the differences among Elasticsearch synonyms, IK tokens, and AliNLP tokens?](#section_3ui_quz_3oa)
-   FAQ about logs
    -   [Can I specify a retention period for the .security indexes of an Elasticsearch cluster?](#section_fhh_6jq_ppy)
    -   [How do I view the logs that are generated before the last seven days?](#section_6qv_ism_v2t)
    -   [I cannot view the search and update logs of an Elasticsearch cluster. What do I do?](#section_erh_hy8_del)
    -   [How do I configure and view the slow logs of an Elasticsearch cluster?](#rza)
    -   [How do I obtain the slow logs of an Elasticsearch cluster on a regular basis?](#rzb)
-   FAQ about data backup and restoration
    -   [Can I restore data from the snapshots of an Elasticsearch cluster to an Elasticsearch cluster of a different version?](#section_czy_aqx_qu2)
    -   [When I back up data for an Elasticsearch cluster, the system displays a message indicating that the cluster is unhealthy. What do I do?](#section_8bi_k19_zdi)
    -   [I enable the Auto Snapshot feature but do not specify shared Object Storage Service \(OSS\) repositories for an Elasticsearch cluster. Are snapshots created?](#section_pj5_ckd_rdv)
    -   [When I restore data from snapshots, the destination Elasticsearch cluster displays a message. This message indicates that shards are abnormal. After I run the POST /\_cluster/reroute?retry\_failed=true command to reroute the shards, the issue persists. What do I do?](#section_24w_qo5_308)
    -   [Can I export data from an Elasticsearch cluster to my on-premises machine?](#backupa)
-   FAQ about cluster monitoring and alerting
    -   [How do I use the email notification feature of X-Pack Watcher?](#section_268_v23_5fr)
    -   [What do I do if the system reports an alert indicating that memory cannot be allocated to the garbage collector?](#section_m0x_9og_rkr)
-   FAQ about access to clusters
    -   [How do I use a client to access an Alibaba Cloud Elasticsearch cluster? What is the difference between access to an Alibaba Cloud Elasticsearch cluster and access to an open source Elasticsearch cluster?](#section_id1_eed_y9x)
    -   [When I use a client to access an Elasticsearch cluster, can I disable the basic authentication feature?](#section_cwg_j34_x6v)
    -   [I purchase an Elastic Compute Service \(ECS\) instance that resides in the same VPC as but a different zone from an Elasticsearch cluster. Can I use the ECS instance to access the Elasticsearch cluster over an internal network?](#visita)
    -   [How do I access an Elasticsearch cluster over the Internet?](#visitb)

## When I purchase an Elasticsearch cluster, I performed an incorrect configuration. How do I modify the configuration after the cluster is created?

If you find that the configurations of your cluster do not meet your expectations after the cluster is created, you can refer to the methods provided in the following table to modify the configurations.

**Warning:** If you are required to cancel the subscription of or release your cluster, you must first back up your data. For more information about how to back up data, see [Create manual snapshots and restore data from manual snapshots](/intl.en-US/Elasticsearch Instances Management/Data backup/Create manual snapshots and restore data from manual snapshots.md). After the cluster subscription cancellation or cluster release, your data stored on the cluster is deleted and cannot be restored.

|Configuration item|Modification method|
|------------------|-------------------|
|Billing method|If you purchased a pay-as-you-go cluster, you can switch the billing method of the cluster to subscription. For more information, see [t1882924.md\#](/intl.en-US/Pricing/Change billing methods/Switch the billing method of a cluster from pay-as-you-go to subscription.md). |
|Version|This configuration item can be modified if one of the following conditions is met:-   The version of the cluster that you purchased is V5.5.3, and you want to upgrade the version to V5.6.16.
-   The version of the cluster that you purchased is V5.6.16, and you want to upgrade the version to V6.3.2.
-   The version of the cluster that you purchased is V6.3.2, and you want to upgrade the version to V6.7.0.

For more information about how to upgrade the version of a cluster, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade version/Upgrade the version of a cluster.md). For other types of version upgrades, we recommend that you cancel the subscription of or release your cluster and purchase another cluster of the desired version. |
|Region|You cannot modify this configuration item. We recommend that you cancel the subscription of or release your cluster and purchase another cluster based on your business requirements.|
|Zone|You can migrate nodes to the desired zone. For more information, see [Migrate nodes in a zone](/intl.en-US/Elasticsearch Instances Management/Data migration/Migrate nodes in a zone.md). **Note:** Before you migrate nodes, make sure that your cluster is in the Active state. |
|Number of zones|You cannot modify this configuration item. We recommend that you cancel the subscription of or release your cluster and purchase another cluster based on your business requirements.|
|Specifications|You can modify this configuration item. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).|
|Storage type|You can modify this configuration item. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).|
|Cloud disk encryption|You cannot modify this configuration item. We recommend that you cancel the subscription of or release your cluster and purchase another cluster based on your business requirements.|
|Storage space per node|You can modify this configuration item. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).|
|Number of data nodes|You can modify this configuration item. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).|
|Network type, VPC, and vSwitch|You cannot modify these configuration items. We recommend that you cancel the subscription of or release your cluster and purchase another cluster based on your business requirements. **Note:** The network type of an Elasticsearch cluster can only be VPC. |
|Username|The default username is elastic. You cannot modify this configuration item. You can create a user in the Kibana console and grant the required permissions to the user. For more information, see [Create a role](/intl.en-US/RAM/Manage Kibana role/Create a role.md) and [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md).|
|Password|You can modify this configuration item. For more information, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|

The preceding table provides only some configuration items. You can go to the cluster configuration page to view other configuration items. For more information about how to modify these configuration items, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).

## What are the mappings between versions on the Elasticsearch buy page and specific Elasticsearch versions?

|Version on the buy page|Specific version|
|-----------------------|----------------|
|7.10|7.10.0|
|7.7|7.7.1|
|7.4|7.4.0|
|6.8|6.8.6|
|6.7|6.7.0|
|6.3|6.3.2|
|5.6|5.6.16|
|5.5|5.5.3|

If you have a self-managed Elasticsearch cluster, we recommend that you select a version that is nearest to the version of the cluster when you purchase an Alibaba Cloud Elasticsearch cluster. For example, you can select a version whose minor version is nearest to that of your self-managed Elasticsearch cluster. If you do not have a self-managed Elasticsearch cluster, we recommend that you select the latest version.

## When I purchase an Elasticsearch cluster, no VPCs are available. What do I do?

You must check whether the RAM user you are using has the required permissions. We recommend that you attach the **AliyunElasticsearchFullAccess** and **AliyunVPCFullAccess** policies to the RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM/Grant permissions to a RAM user.md).

## When I purchase an Elasticsearch cluster, I select a VPC, but no vSwitches are available. What do I do?

You must check whether vSwitches are created in the selected region. If no vSwitches are created in the selected region, you must create one. For more information, see [Create an IPv4 VPC](/intl.en-US/Quick Start/Create an IPv4 VPC.md).

## After I cancel the subscription of or release an Elasticsearch cluster, I purchase another cluster. Does the endpoint of the new cluster remain the same as that of the original cluster?

No, the endpoints are different. After you purchase the new cluster, we recommend that you modify the client code and cancel the subscription of or release the original cluster to avoid service interruptions.

## How do I release an Elasticsearch cluster?

On the Elasticsearch Clusters page, find the cluster that you want to release and choose **More** \> **Release** in the Actions column. For more information, see [Release a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Release a cluster.md).

## When does the system release an Elasticsearch cluster after the cluster is suspended?

The system releases the cluster one day after the cluster is suspended. After the cluster is released, all data stored on the cluster is permanently deleted and cannot be recovered. For more information, see [Overdue payments and cluster release](/intl.en-US/Pricing/Overdue payments and cluster release.md).

## Can I purchase an Elasticsearch cluster that has only one node?

No, an Elasticsearch cluster must have a minimum of two data nodes. For more information, see [Parameters on the buy page](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Parameters on the buy page.md).

## When I purchase an Elasticsearch cluster, resources of a specific category are sold out. What do I do?

Take one of the following measures:

-   Select another region.
-   Select another zone.
-   Select another category.

If the resources that you want to purchase are still unavailable after you take all of the preceding measures, try again later. Resources are dynamic. If resources are insufficient, Alibaba Cloud replenishes the resources at the earliest opportunity.

## Can I upgrade or downgrade the version of an Elasticsearch cluster?

Upgrades are supported, whereas downgrades are not supported. You can upgrade the versions of clusters only from V5.5.3 to V5.6.16, from V5.6.16 to V6.3.2, or from V6.3.2 to V6.7.0. For more information, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade version/Upgrade the version of a cluster.md).

If you want to perform upgrades between other versions or downgrades, purchase an Elasticsearch cluster of the desired version. Then, migrate data from the original cluster to the new cluster and cancel the subscription of or release the original cluster.

## Can I log on to an Elasticsearch cluster over SSH and modify the configuration of the cluster?

No, for security purposes, you are not allowed to log on to your Elasticsearch cluster over SSH. If you want to modify the configuration of your cluster, use the cluster configuration feature of Elasticsearch. For more information, see [Overview](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Overview.md).

## Is Logstash V6.7 compatible with Elasticsearch V6.3?

Yes, for more information, see [Compatibility matrixes](/intl.en-US/Product Introduction/Compatibility matrixes.md).

## Can Elasticsearch be used as a data source of Quick BI?

No, you can use Kibana to analyze data and present analysis results.

## Does Elasticsearch support scoring plug-ins?

Yes, when you create an index, Elasticsearch allows you to create a tokenizer. This way, when you search for data, Elasticsearch uses a scoring plug-in to sort search results by score. For more information, see [Step 5: Search for data](/intl.en-US/Elasticsearch Instances Management/Quick start.md).

## Does Elasticsearch support LDAP?

No, if you want to use LDAP to authenticate requests that are sent to your Elasticsearch cluster, you must deploy an on-premises Elasticsearch cluster of the same version and use this cluster to conduct an authentication test. If LDAP runs as expected, send the related LDAP configurations to Alibaba Cloud Elasticsearch technical engineers. Then, the engineers configure your cluster to support LDAP based on the configurations that you sent. For more information, see [Integrate X-Pack with LDAP](/intl.en-US/Best Practices/Elasticsearch applications/Cluster management/Integrate X-Pack with LDAP.md).

## Does Alibaba Cloud provide Elasticsearch SDK for Java?

Yes, different Elasticsearch versions use different SDKs. For more information, see [Java API](/intl.en-US/Developer Guide/Java API/Overview.md).

## How do I view the kernel version of an Elasticsearch cluster?

By default, Elasticsearch clusters use the kernel of the latest version. For more information about kernel versions, see [AliES release notes](/intl.en-US/AliES Kernel/AliES release notes.md). If your cluster does not use the kernel of the latest version, the **A new kernel patch is available** message appears on the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page of your cluster. You can click the message to view the current kernel version of your cluster.

![View the kernel version of a cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8718158061/p183995.png)

## How long is required to restart an Elasticsearch cluster or node?

When you restart an Elasticsearch cluster or node, the system displays the required time. The time is estimated based on the specifications, data structure, and data volume of the cluster or node. In most cases, a few hours are required to restart a cluster. For more information, see [Restart a cluster or node](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Restart a cluster or node.md).

## Does the system restart an Elasticsearch cluster after I enable or disable the Public Network Access feature for the cluster?

No, only the status of the Public Network Access feature changes. This does not affect your cluster.

## The CPU utilization and loads of some nodes in an Elasticsearch cluster are normal, whereas other nodes are in the idle state. What do I do?

This issue is caused by unbalanced loads on the cluster. Unbalanced loads may be caused by several reasons, which include inappropriate shard settings, uneven segment sizes, unseparated hot and cold data, and persistent connections that are used for Service Load Balancer \(SLB\) instances and multi-zone architecture. Resolve the issue based on the actual scenario. For more information, see [Unbalanced loads on a cluster]().

**Note:** Before you resolve the issue, check the specifications of your cluster. If the specifications of your cluster are 1 vCPU and 2 GiB of memory, upgrade the specifications to 2 vCPUs and 4 GiB of memory or higher. The specifications of 1 vCPU and 2 GiB of memory are used only for tests. For more information about how to upgrade the specifications, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).

## How do I plan resources before I use Elasticsearch, such as cluster specifications, the number of shards, and the size of each shard?

Evaluate the specifications and storage capacity of your Elasticsearch cluster. For more information, see [Evaluate specifications and storage capacity](). You can purchase an Elasticsearch cluster or upgrade the configuration of the cluster based on the evaluation results.

## How do I view the configuration of an Elasticsearch cluster?

You can view the configuration of your Elasticsearch cluster on the Basic Information page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).

When you use Transport Client to access an Elasticsearch cluster, set the `cluster.name` parameter to the ID of your cluster. For more information, see [Transport Client \(5.x\)](/intl.en-US/Developer Guide/Java API/Transport Client (5.x).md).

## Are services affected when I modify the configuration of an Elasticsearch cluster?

The system restarts the cluster after you modify its configuration. The system uses the rolling restart method to restart a cluster. Before the restart, make sure that the cluster is in the Active state \(indicated by the color green\), each index has at least one replica shard for each primary shard, and resource usage is not high. For example, the value of NodeCPUUtilization\(%\) is about 80%, that of NodeHeapMemoryUtilization is about 50%, and that of NodeLoad\_1m is less than the number of vCPUs of the current node. If all the conditions are met, the cluster can still provide services during the restart. You can view the resource usage on the [Cluster Monitoring](/intl.en-US/Elasticsearch Instances Management/Cluster monitoring and alerting/Monitoring metrics.md) page. However, we recommend that you modify the configuration of your cluster during off-peak hours.

## Can I change the cloud disk type of an Elasticsearch cluster?

No, if you want to change the cloud disk type of your cluster, purchase another cluster based on your requirements and migrate data from the original cluster to the new cluster. Then, cancel the subscription of or release the original cluster. For more information about how to migrate data, see [Configure a shared OSS repository](/intl.en-US/Elasticsearch Instances Management/Data backup/Configure a shared OSS repository.md).

## Can I convert other types of nodes in an Elasticsearch cluster to warm nodes?

No, the conversion can cause your cluster to be unstable. For more information, see ["Hot-Warm" Architecture in Elasticsearch 5.x](https://www.elastic.co/cn/blog/hot-warm-architecture-in-elasticsearch-5-x?spm=a2c4g.11186623.2.14.22557ad9E8iQ2N).

## Can I downgrade the specifications of an Elasticsearch cluster? If yes, how do I do?

No, you cannot downgrade the specifications of the cluster. However, you can scale in the cluster. For more information, see [Scale in a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Scale in a cluster.md).

## How do I modify the configuration of an Elasticsearch cluster to ensure that services run as expected when a temporary business surge occurs?

We recommend that you add nodes to the cluster when the temporary business surge occurs and remove the nodes after the business surge. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md) and [Scale in a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Scale in a cluster.md). For the changes to take effect, the system restarts the cluster. Before the restart, take note of the following items:

-   The cluster is in the Active state \(indicated by the color green\).
-   Each index of the cluster has at least one replica shard for each primary shard, and the resource usage of the cluster is not high. For example, the value of NodeCPUUtilization\(%\) is about 80%, that of NodeHeapMemoryUtilization is about 50%, and that of NodeLoad\_1m is less than the number of vCPUs of the current node. You can view the resource usage on the Cluster Monitoring page of the cluster.

## When I upgrade the configuration of an Elasticsearch cluster, the system displays the "UpgradeVersionMustFromConsole" error message. What do I do?

The error message is returned because the version change does not meet requirements. You can upgrade the versions of clusters only from V5.5.3 to V5.6.16, from V5.6.16 to V6.3.2, or from V6.3.2 to V6.7.0.

## How long is required to upgrade the version of an Elasticsearch cluster?

The required time is determined by the data volume, data structure, and specifications of your cluster. The version upgrade requires about 1 hour.

## Are services affected when I upgrade the version of an Elasticsearch cluster?

When you upgrade the version of an Elasticsearch cluster, you can still read data from or write data to the cluster but cannot make other changes. We recommend that you perform a version upgrade during off-peak hours. For more information about the precautions and procedure for a version upgrade, see [Upgrade the version of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade version/Upgrade the version of a cluster.md).

## Can I use the YML configuration file of an Elasticsearch cluster to configure the http.max\_content\_length and discovery.zen.ping\_timeout parameters?

You are not allowed to configure the two parameters. If you want to add these parameters to the configuration file, contact Alibaba Cloud Elasticsearch technical engineers. Before you add the parameters, make sure that the parameter settings are correct and you accept the impact caused by parameter modifications. If the parameter settings are incorrect, the system fails to perform a rolling restart for the cluster.

**Note:** In most cases, you do not need to change the settings of the following parameters: discovery.zen.ping\_timeout, discovery.zen.fd.ping\_timeout, discovery.zen.fd.ping\_interval, and discovery.zen.fd.ping\_retries.

## Can I switch the VPC of an Elasticsearch cluster?

No, you can purchase an Elasticsearch cluster in the desired VPC and migrate data from the original cluster to the new cluster. Then, cancel the subscription of or release the original cluster.

## Can I change the JVM parameter settings of an Elasticsearch cluster?

Alibaba Cloud Elasticsearch clusters use JVM parameter settings that are recommended by open source Elasticsearch. We recommend that you do not change the settings. By default, JVM heap memory is half of cluster memory.

## How do I update dictionary content when I use the IK analysis plug-in?

You can use the standard update or rolling update feature of the IK analysis plug-in to update dictionary content. For more information, see [Use the analysis-ik plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the analysis-ik plug-in.md).

## When I use the IK analysis plug-in, the system displays the "ik startOffset" error message. What do I do?

The error message is returned because of an Elasticsearch V6.7 bug. You must restart your cluster. For more information, see [Restart a cluster or node](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Restart a cluster or node.md).

## The IK dictionary files on my on-premises machine are lost. Can I retrieve them on the cluster management page?

No, you can only delete or update dictionary files on the cluster management page. We recommend that you download the [official main and stopword dictionary files](https://github.com/medcl/elasticsearch-analysis-ik/releases?spm=a2c4g.11186623.2.14.29f41736MORIEU). Then, change the tokens in the files to those in your system dictionary file and upload the files to your cluster.

## How do I apply updated IK dictionaries to existing data?

You must perform a reindex operation. If indexes are configured with IK tokens, the updated dictionaries apply only to new data in these indexes. If you want to apply the updated dictionaries to all the data in these indexes, you must perform a reindex operation. For more information, see [Configure a remote reindex whitelist](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md).

## Is there a specific threshold for full GC?

Full GC is used to clean the entire heap memory. Whether full GC is correctly performed needs to be analyzed based on the service latency, heap memory size before full GC, and heap memory size after full GC. The CMS collector starts to collect garbage when the memory usage is 75%. This is because some space is reserved for burst traffic.

## Can I remove plug-ins that are not used?

You can remove only some plug-ins. On the Plug-ins page of your Elasticsearch cluster, you can view plug-ins that can be removed on the Built-in Plug-ins tab. If Remove is displayed in the Actions column of a plug-in, the plug-in can be removed. For more information about how to remove a plug-in, see [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Install and remove a built-in plug-in.md).

## Are the dictionaries provided by the IK analysis plug-in of Alibaba Cloud Elasticsearch the same as the dictionaries provided by the IK analysis plug-in of open source Elasticsearch?

Yes, for more information, see [IK Analysis for Elasticsearch](https://github.com/medcl/elasticsearch-analysis-ik).

## Can a custom plug-in access an external network, such as reading dictionary files on GitHub?

No, if you want your Elasticsearch cluster to access external files, upload the files to OSS and connect your Elasticsearch cluster to OSS.

## Does a custom plug-in support the rolling update method?

No, if you want a custom plug-in to support this method, configure the plug-in based on the rolling update method of the IK analysis plug-in. For more information, see [IK Analysis for Elasticsearch](https://github.com/medcl/elasticsearch-analysis-ik).

## How do I configure the analysis-aliws plug-in? What is the format of the dictionary file for this plug-in?

For more information about how to configure the plug-in, see [Use the analysis-aliws plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the analysis-aliws plug-in.md).

The dictionary file must meet the following requirements:

-   Name: aliws\_ext\_dict.txt.
-   Encoding format: UTF-8.
-   Content: Each row contains one word and ends with \\n \(line break in UNIX or Linux\). No whitespace characters are used before or after this word. If the dictionary file is generated in Windows, you must use the dos2unix tool to convert the file before you upload it.

## What are the differences among Elasticsearch synonyms, IK tokens, and AliNLP tokens?

|Token type|Usage|Description|Supported file type|Tokenizer and analyzer|
|----------|-----|-----------|-------------------|----------------------|
|Synonym|You can upload a synonym dictionary file on the Cluster Configuration page of your cluster to enable the cluster to use it.|After you write several synonyms to the file, the system displays all the synonyms when you query one of them.|The synonym dictionary file must be a TXT file encoded in UTF-8.|Custom tokenizer and analyzer|
|IK token|The IK tokens are used based on the analysis-ik plug-in.|The system splits a paragraph based on the main.dic file. If you send a query request that contains one or more words split from the paragraph, the system returns the entire paragraph in the query result. The analysis-ik plug-in also provides a stopword file named stop.dic. The query result does not include the stopwords in the stop.dic file You can view the dictionary file from the [official documentation](https://github.com/medcl/elasticsearch-analysis-ik).|The main and stopword dictionary files must be DIC files encoded in UTF-8.|Tokenizer:-   ik\_smart
-   ik\_max\_word |
|AliNLP token|The AliNLP tokens are used based on the analysis-aliws plug-in.|The analysis-aliws plug-in works in a similar way as the analysis-ik plug-in, but the analysis-aliws plug-in does not provide a separate stopword dictionary file. Stopwords are integrated into the main dictionary file aliws\_ext\_dict.txt. The file is invisible to you. In addition, you are not allowed to customize stopwords.|The dictionary file name must be aliws\_ext\_dict.txt. The file must be encoded in UTF-8.|-   Analyzer: aliws, which does not return function words, function phrases, or symbols
-   Tokenizer: aliws\_tokenizer |

## Can I specify a retention period for the .security indexes of an Elasticsearch cluster?

No, Elasticsearch does not automatically delete expired indexes. You must manually delete the expired .security indexes. For more information, see [Step 6: \(Optional\) Delete the index](/intl.en-US/Elasticsearch Instances Management/Quick start.md).

## How do I view the logs that are generated before the last seven days?

You can call the ListSearchLog API operation to obtain all the logs that you require. For more information, see [ListSearchLog](/intl.en-US/API Reference/Elasticsearch instances/Query logs/ListSearchLog.md).

## I cannot view the search and update logs of an Elasticsearch cluster. What do I do?

You can configure slow logs and reduce the timestamp precision of log entries. For more information, see [Configure slow logs](/intl.en-US/Elasticsearch Instances Management/Query logs.md).

## How do I configure and view the slow logs of an Elasticsearch cluster?

By default, Elasticsearch logs only read and write operations that require 5 seconds to 10 seconds to complete as slow logs. You can log on to the [Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) of the cluster and run the related command to reduce the timestamp precision of log entries. This helps capture more logs. For more information, see [Configure slow logs](/intl.en-US/Elasticsearch Instances Management/Query logs.md).

**Note:** You are not allowed to change the format of slow logs.

## How do I obtain the slow logs of an Elasticsearch cluster on a regular basis?

You can call the ListSearchLog API operation to obtain the slow logs of your cluster on a regular basis. For more information, see [ListSearchLog](/intl.en-US/API Reference/Elasticsearch instances/Query logs/ListSearchLog.md).

## Can I restore data from the snapshots of an Elasticsearch cluster to an Elasticsearch cluster of a different version?

For automatic snapshots, you can restore data from the snapshots only to the original cluster. For more information, see [Create automatic snapshots and restore data from automatic snapshots](/intl.en-US/Elasticsearch Instances Management/Data backup/Create automatic snapshots and restore data from automatic snapshots.md).

For manual snapshots, you can restore data from the snapshots to a cluster other than the original cluster. We recommend that you use a destination cluster whose version is the same as the original cluster. If the versions are different, compatibility issues may occur. For more information, see [Create manual snapshots and restore data from manual snapshots](/intl.en-US/Elasticsearch Instances Management/Data backup/Create manual snapshots and restore data from manual snapshots.md).

## When I back up data for an Elasticsearch cluster, the system displays a message indicating that the cluster is unhealthy. What do I do?

When an Elasticsearch cluster is unhealthy, you cannot use the Auto Snapshot feature and specify shared OSS repositories. You can purchase an OSS bucket that resides in the same region as your Elasticsearch cluster. Then, create an OSS repository and manually create snapshots. For more information, see [Create manual snapshots and restore data from manual snapshots](/intl.en-US/Elasticsearch Instances Management/Data backup/Create manual snapshots and restore data from manual snapshots.md).

## I enable the Auto Snapshot feature but do not specify shared OSS repositories for an Elasticsearch cluster. Are snapshots created?

Elasticsearch provides an OSS bucket for your cluster by default. You can log on to the Kibana console of your cluster and run the `GET _snapshot/aliyun_auto_snapshot/_all` command to obtain automatic snapshots. For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

## When I restore data from snapshots, the destination Elasticsearch cluster displays a message. This message indicates that shards are abnormal. After I run the `POST /_cluster/reroute?retry_failed=true` command to reroute the shards, the issue persists. What do I do?

The following figure shows the issue.

![Data restoration issue](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8718158061/p179003.png)

Delete the problematic index and call the \_restore API to restore it. You must add the max\_restore\_bytes\_per\_sec parameter to the request. This parameter is used to limit the restoration rate. The default value of this parameter is 40mb. This value indicates that the index is restored at a speed of 40 MB per second.

```
POST /_snapshot/aliyun_snapshot_from_instanceId/es-cn-instanceId_datetime/_restore
{
    "indices": "myIndex",
    "settings": {
    "max_restore_bytes_per_sec" : "150mb" 
    }
}
```

**Note:** You can also add the following parameters:

-   compress: specifies whether to enable data compression. Default value: true.
-   max\_snapshot\_bytes\_per\_sec: specifies the rate at which snapshots are created for each node. Default value: 40mb.

## Can I export data from an Elasticsearch cluster to my on-premises machine?

Yes, you can use the data backup feature provided by Elasticsearch to export data. For more information, see [Data backup overview](/intl.en-US/Elasticsearch Instances Management/Data backup/View the snapshot feature.md). You can create snapshots, store them in OSS, and then download objects from OSS. For more information, see [Download objects](/intl.en-US/Quick Start/OSS console/Download objects.md).

## How do I use the email notification feature of X-Pack Watcher?

You can configure specific actions for X-Pack Watcher. For more information, see [Watcher settings in Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/actions-email.html).

**Note:** X-Pack Watcher of Elasticsearch cannot directly access the Internet. You must use the internal endpoint of an Elasticsearch cluster to access the Internet. Therefore, you must create an ECS instance that can access both the Internet and the Elasticsearch cluster. Then, use the ECS instance as a proxy to perform actions. For more information, see [Configure X-Pack Watcher](/intl.en-US/Elasticsearch Instances Management/Cluster monitoring and alerting/Configure X-Pack Watcher.md).

## What do I do if the system reports an alert indicating that memory cannot be allocated to the garbage collector?

Possible causes include heavy loads, high query QPS, or large amounts of data to write. Troubleshoot the issue based on the following instructions:

-   Heavy loads: For more information, see [High disk usage and read-only indexes]().
-   High query QPS or large amounts of data to write: We recommend that you install the aliyun-qos plug-in on your Elasticsearch cluster to implement read/write throttling. For more information, see [Use the aliyun-qos plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Use the aliyun-qos plug-in.md).

    **Note:** For image retrieval, we recommend that you install the aliyun-knn plug-in on your cluster and plan your cluster and indexes. For more information, see [Use the aliyun-knn plug-in]().


## How do I use a client to access an Alibaba Cloud Elasticsearch cluster? What is the difference between access to an Alibaba Cloud Elasticsearch cluster and access to an open source Elasticsearch cluster?

Access an Alibaba Cloud Elasticsearch cluster by using its internal or public endpoint. Access an open source Elasticsearch cluster by using its address. For more information, see [Use a client to access an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Developer Guide/Use a client to access an Alibaba Cloud Elasticsearch cluster.md).

## When I use a client to access an Elasticsearch cluster, can I disable the basic authentication feature?

No, the basic authentication feature is a Kibana authentication mechanism provided by the built-in Elasticsearch plug-in X-Pack. Therefore, you cannot disable the feature.

## I purchase an ECS instance that resides in the same VPC as but a different zone from an Elasticsearch cluster. Can I use the ECS instance to access the Elasticsearch cluster over an internal network?

Yes, you can use an ECS instance to access an Elasticsearch cluster over an internal network if they reside in the same VPC.

## How do I access an Elasticsearch cluster over the Internet?

You can access the cluster over the Internet by using its public endpoint and configuring a public IP address whitelist. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md). When you access the cluster, you must configure parameters, such as the domain name, username, and password. For more information, see [Use a client to access an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Developer Guide/Use a client to access an Alibaba Cloud Elasticsearch cluster.md).

