---
keyword: [use Filebeat to collect the logs of an ACK cluster, use Filebeat to collect the logs of a Kubernetes cluster]
---

# Collect the logs of an ACK cluster

Alibaba Cloud Filebeat can be used to collect the logs of Container Service for Kubernetes \(ACK\) clusters and send the collected logs to Alibaba Cloud Elasticsearch for analysis and presentation. This topic describes how to configure Filebeat to collect the logs of an ACK cluster. It also provides information about the containers of Filebeat.

-   An Alibaba Cloud Elasticsearch cluster is created.

    For more information, see [Create a cluster](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Create a cluster.md) or [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

-   A prefix is customized for the names of indexes created by using the Auto Indexing feature.

    To avoid a conflict between the alias of the index that is generated during a rollover and the index name, we recommend that you customize the filebeat-\* prefix for index names. You can specify **+.\*,+filebeat-\*,-\*** in the filebeat-\* field. For more information, see [Configure the YML file](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md).

    ![Customize a prefix for index names](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4697316161/p231744.png)

    **Note:** If you enable the **Rolling Update** feature when you [configure lifecycle management for the index](#step_iih_doy_mcu), you must disable the Auto Indexing feature to avoid a conflict between the alias of the index after the rolling update and the index name. If you do not enable the **Rolling Update** feature for the index, you must enable the Auto Indexing feature. We recommend that you customize a prefix for index names.

-   The permissions on Beats and ACK clusters are granted to a RAM user.

    For more information, see [Create a custom policy](/intl.en-US/RAM/Create a custom policy.md) and [Assign RBAC roles to a RAM user](/intl.en-US/User Guide for Kubernetes Clusters/Authorization management/Assign RBAC roles to a RAM user.md).

-   An ACK cluster is created, and a pod is created in the cluster. In this topic, a NGINX container is used.

    For more information, see [Create a managed Kubernetes cluster](/intl.en-US/User Guide for Kubernetes Clusters/Cluster/Create Kubernetes clusters/Create a managed Kubernetes cluster.md).


## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select a region. In the left-side navigation pane, click **Beats Data Shippers**.

3.  If this is the first time you go to the **Beats Data Shippers** page, click **Confirm** in the Confirm Service Authorization message to authorize the system to create a service-linked role for your account.

    ![Confirm Service Authorization](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8144790261/p268158.png)

    **Note:** When Beats collects data from various data sources, it depends on the service-linked role and the rules specified for the role. Do not delete the service-linked role. Otherwise, the use of Beats is affected. For more information, see [Overview of the Elasticsearch service-linked role](/intl.en-US/RAM/Overview of the Elasticsearch service-linked role.md).

4.  In the **Create Shipper** section, move the pointer over **Filebeat** and click **ACK Logs**.

5.  In the **Select Destination Elasticsearch Cluster** step, configure the following parameters. Then, click **Next**.

    ![Select Destination Elasticsearch Cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4697316161/p231821.png)

    |Parameter|Description|
    |---------|-----------|
    |**Shipper Name**|The name of the shipper. Enter a name for the shipper. The name must be 1 to 30 characters in length and can contain letters, digits, underscores \(\_\), and hyphens \(-\). The name must start with a letter.|
    |**Version**|Set Version to **6.8.13**, which is the only version supported by Filebeat.|
    |**Output**|The destination for the data collected by Filebeat. The destination is the Elasticsearch cluster you created. The protocol must be the same as that of the selected Elasticsearch cluster.|
    |**Username/Password**|The username and password used to access the Elasticsearch cluster. The default username is elastic. The password is specified when you create the Elasticsearch cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|
    |**Enable Kibana Monitoring**|Determine whether to monitor the metrics of Filebeat. If you select **Elasticsearch** for **Output**, the Kibana monitor uses the same Alibaba Cloud Elasticsearch cluster as **Output**.|
    |**Enable Kibana Dashboard**|Determine whether to enable the default Kibana dashboard. Alibaba Cloud Kibana is configured in a virtual private cloud \(VPC\). You must enable the Private Network Access feature for Kibana on the Kibana configuration page. For more information, see [Configure an IP address whitelist for access to the Kibana console over the Internet or an internal network](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure an IP address whitelist for access to the Kibana console over the Internet
         or an internal network.md).|

6.  In the **Configure Collection Object** step, configure the collection object.

    1.  Select an option from the **Source ACK Cluster** drop-down list.

        **Note:** You must select a running ACK cluster that resides in the same VPC as the Elasticsearch cluster and is not a managed edge ACK cluster. For more information, see [ACK@Edge overview](/intl.en-US/User Guide for Edge Container Service/ACK@Edge overview.md).

    2.  Click **Install** to install ES-operator for the ACK cluster. Beats depends on ES-operator.

        If the **Install** button is not displayed, ES-operator has been installed. If the **Install** button disappears after you install ES-operator, the installation is successful.

    3.  Click **Create Collection Object** in the lower-left corner to configure the collection object. You can configure multiple collection objects.

        ![Create Collection Object](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4697316161/p231833.png)

        |Parameter|Description|
        |---------|-----------|
        |**Object Name**|The name of the collection object. You can create multiple collection objects. The names of the collection objects must be unique.|
        |**Namespace**|The namespace of the ACK cluster where the pod from which you want to collect logs is deployed. If you do not specify a namespace when you create the pod, the namespace default is used by default.|
        |**Pod Label**|Add a label to the pod. If you add multiple labels to the pod, the labels have logical AND relations. **Note:** You can delete the labels that are added to the pod only if you add a minimum of two labels to it. |
        |**Container Name**|The full name of the container. If this parameter is not specified, Filebeat collects logs from all the containers in the namespace that comply with the labels added to the pod.|

        **Note:** For more information about how to obtain the configurations of the pod, such as the namespace, labels, and name, see [Manage pods](/intl.en-US/User Guide for Kubernetes Clusters/Application management/Workloads/Manage pods.md).

    4.  Click **Next**.

7.  In the **Configure Log Collection** step, click **Add Log Collection Configuration** in the lower-left corner to configure log collection information. You can specify multiple containers from which you want to collect logs. Click **Next**.

    ![Configure Log Collection](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4697316161/p232001.png)

    |Parameter|Description|
    |---------|-----------|
    |**Log Name**|You can specify multiple containers for each pod. Each container corresponds to a log name. Each log name must be unique. A log name can be a part of an index name and used for subsequent collection output.|
    |**Configure Shipper**|A general template for collecting logs from a Docker container is used. The [Autodiscover](https://www.elastic.co/guide/en/beats/filebeat/7.10/configuration-autodiscover.html#_kubernetes) provider is integrated into the log collection configuration. The collection configuration supports [Docker input](https://www.elastic.co/guide/en/beats/filebeat/6.8/filebeat-input-docker.html#filebeat-input-docker).    -   type: the input type. If Filebeat collects logs from a container, the value of this parameter is docker. The value of this parameter varies based on the input type. For more information, see [Configure inputs](https://www.elastic.co/guide/en/beats/filebeat/7.10/configuration-filebeat-options.html).
    -   combine\_partial: Enable partial message joining. For more information, see [Docker input \(combine\_partial\)](https://www.elastic.co/guide/en/beats/filebeat/7.10/filebeat-input-docker.html#_combine_partial).
    -   container ids: the IDs of the Docker containers from which you want to read logs. For more information, see [Docker input \(containers.ids\)](https://www.elastic.co/guide/en/beats/filebeat/6.8/filebeat-input-docker.html#config-container-ids).
    -   fields.k8s\_container\_name: Add the k8s\_container\_name field to the output information of Filebeat to reference the variable $\{data.kubernetes.container.name\}.

**Note:** The [Autodiscover](https://www.elastic.co/guide/en/beats/filebeat/7.10/configuration-autodiscover.html#_kubernetes) provider is integrated into the Docker container configuration. You can reference the configuration of the [Autodiscover](https://www.elastic.co/guide/en/beats/filebeat/7.10/configuration-autodiscover.html#_kubernetes) provider to configure the Docker container.

    -   fileds.k8s\_node\_name: Add the k8s\_node\_name field to the output information of Filebeat to reference the variable $\{data.kubernetes.node.name\}.
    -   fields.k8s\_pod: Add the k8s\_pod field to the output information of Filebeat to reference the variable $\{data.kubernetes.pod.name\}.
    -   fileds.k8s\_pod\_namespace: Add the k8s\_pod\_namespace field to the output information of Filebeat to reference the variable $\{data.kubernetes.namespace\}.
    -   fields\_under\_root: If this parameter is set to true, the fields are stored as top-level fields in the output document. For more information, see [Docker input \(fields\_under\_root\)](https://www.elastic.co/guide/en/beats/filebeat/6.8/filebeat-input-docker.html#fields-under-root-docker).
**Note:**

    -   If a general template does not meet your requirements, you can modify the configurations. For more information, see [Filebeat Docker input](https://www.elastic.co/guide/en/beats/filebeat/6.8/filebeat-input-docker.html#fields-under-root-docker).
    -   Only one Docker container can be configured for each shipper. If you want to configure multiple Docker containers, click **Add Log Collection Configuration** to perform the operation. |

8.  In the **Manage Index Storage** step, enable and configure the Index Storage Management for Collected Data feature based on your business requirements.

    After you enable the Index Storage Management for Collected Data feature, click **Add Management Policy** to create and configure the index management policy based on the following parameters. You can configure multiple index management policies.

    ![Manage Index Storage](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4697316161/p231865.png)

    |Parameter|Description|
    |---------|-----------|
    |**Policy Name**|You can specify multiple containers for each pod. Each container corresponds to a log file name. A log file name can be a part of an index name and can be used for subsequent collection output.|
    |**Log Name**|The name of the log file that you want to associate with the policy. You must specify a minimum of one name. You can specify multiple log files for a management policy, but you can specify each log file for only one management policy.|
    |**Maximum Storage Space**|If the disk space consumed by the index \(including replica shards\) reaches the value of this parameter, the system deletes old data to save disk space.|
    |**Lifecycle Management**|Specifies whether to turn on Lifecycle Management for the index. After you turn on Lifecycle Management, the system separates hot data from cold data for data nodes and automatically deletes old data. For more information, see [Use ILM to separate hot data from cold data](/intl.en-US/Best Practices/Elasticsearch applications/Cluster management/Use ILM to separate hot data from cold data.md) and [Managing the index lifecycle](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/index-lifecycle-management.html). **Note:**

    -   If you turn on Rolling Update, Filebeat writes data to the index named <Log name\>-<Date\>-<Number\>, such as log-web-2021.01.22-000001.
    -   If you do not turn on Rolling Update, Filebeat writes data to the index named filebeat-<Log name\>-<Date\>. |

9.  Click **Enable**.

    After the shipper is enabled, you can view the information of the shipper in the Manage Shippers section. You can also perform the following operations.

    **Note:** The Filebeat resources are deployed in the logging namespace of the ACK cluster.

    |Operation|Description|
    |---------|-----------|
    |**View Configuration**|View information such as the destination Elasticsearch cluster, source ACK cluster, and name of the log collection task of the shipper. You cannot modify the information.|
    |**Modify Configuration**|Modify the collection object, log collection configuration, and index storage management policy.|
    |**More**|Enable, disable, restart, or delete the log collection task. You can also view the information related to the task in the dashboard and helm charts.|


## View the Filebeat resources

[Use kubectl to connect to the ACK cluster](https://www.alibabacloud.com/help/zh/doc-detail/86378.html?spm=a2c5t.11065259.1996646101.searchclickresult.7c71694b9cyhQH) and view the Filebeat resources in the logging namespace.

```
kubectl get pods  -n logging
```

![fig01](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1046242261/p262295.png)

**Warning:** You are not allowed to perform operations on the resources that are deployed in the logging namespace, such as resource deletions. Otherwise, the service of Filebeat is affected.

|pod name|Description|Example|
|--------|-----------|-------|
|Cluster name-binding-Serial number|The container used to manage indexes, such as a container that is used to delete old data on a regular basis.|ct-cn-ew8qx563gu4ng4ot6-binding-7e245-1617347400-c\*\*\*\*|
|Cluster name-policy-Serial number|The policy for rolling updates on indexes.|ct-cn-ew8qx563gu4ng4ot6-policy-696b7-hot-rollover-1g-16173v\*\*\*\*|
|Cluster name-Serial number|The container for which Filebeat is installed.|ct-cn-ew8qx563gu4ng4ot6-q\*\*\*\*|
|es-operator-Serial number|The container for which ES-operator is installed.|es-operator-cb63cc9a6302e4e90aeb2f79adf358b19-56fcd754db-b\*\*\*\*|

