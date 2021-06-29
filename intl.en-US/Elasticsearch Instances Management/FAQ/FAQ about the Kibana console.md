# FAQ about the Kibana console

This topic provides answers to some frequently asked questions about the Kibana console.

-   [How do I log on to the Kibana console? What are the username and password?](#section_nc8_1qm_zrp)
-   [What is the password of the elastic account used for?](#section_bkt_kbk_nc9)
-   [Can I access Internet services, such as Baidu Maps and AMAP, in the Kibana console?](#section_t4r_wg7_ncy)
-   [What is the better way to manage permissions in the Kibana console?](#section_fqw_iih_b8e)
-   [The Kibana console fails to start, and the "Kibana server is not ready yet" message is displayed. What do I do?](#section_0sp_hnf_hp8)
-   [How do I view the information of shards and indexes in the Kibana console?](#section_csk_ofw_thc)
-   [When I use the elastic account to create a user in the Kibana console, the "You do not have permission to manage users" message is displayed. What do I do?](#section_42g_3si_brh)
-   [Can I install custom plug-ins for Kibana?](#section_9mh_c46_eki)

## How do I log on to the Kibana console? What are the username and password?

For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md). The username is elastic. The password is the one you specified when you created your Elasticsearch cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).

## What is the password of the elastic account used for?

**Note:** The elastic account is the administrator account of Alibaba Cloud Elasticsearch. It has full permissions to manage clusters.

When you use one of the following items to access your Elasticsearch cluster, you must specify the password of the elastic account to verify permissions.

-   API or SDK
-   Kibana

## Can I access Internet services, such as Baidu Maps and AMAP, in the Kibana console?

No, you can access only the services that are deployed in virtual private clouds \(VPCs\).

## What is the better way to manage permissions in the Kibana console?

-   We recommend that you create users, assign roles to the users in the Kibana console of your Elasticsearch cluster, and then use the users to perform operations on the cluster. For more information, see [Permissions on indexes](/intl.en-US/RAM/Manage Kibana role/Create a role.md).
-   We recommend that you do not use the elastic account in searches. If the password of the elastic account is disclosed, the security of your cluster may be threatened.
-   Exercise caution when you reset the password of the elastic account. If you use the elastic account to manage your workloads, your requests are rejected due to authentication failures, and your workloads are interrupted after you reset the password.

## The Kibana console fails to start, and the "Kibana server is not ready yet" message is displayed. What do I do?

Cause: System indexes are deleted, or the Kibana node is lost.

Solutions:

-   System indexes are deleted: Restore the deleted system indexes from snapshots. For more information, see [Create automatic snapshots and restore data from automatic snapshots](/intl.en-US/Elasticsearch Instances Management/Data backup/Create automatic snapshots and restore data from automatic snapshots.md).
-   The Kibana node is lost: Delete the indexes that are related to the .kibana\_1 and .kibana indexes. Then, restart the Kibana node in the Kibana console or restart your Elasticsearch cluster. For more information, see [Restart a cluster or node](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Restart a cluster or node.md).

## How do I view the information of shards and indexes in the Kibana console?

-   Run the `GET _nodes/stats` command to view the information of indexes.

    ![Run a command to view the information of indexes and shards](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5318158061/p181570.png)

-   On the **Monitoring** page, view the shards of indexes on a node, including the heap memory usage.

    ![View the information of shards](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5318158061/p181569.png)


## When I use the elastic account to create a user in the Kibana console, the "You do not have permission to manage users" message is displayed. What do I do?

The following figure shows the error message that is displayed.

![Error message](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5318158061/p181589.png)

Solution:

1.  Check whether the certificate expires in the Kibana console.

    ```
    GET _license
    ```

    -   Yes: Submit a ticket to Alibaba Cloud Elasticsearch technical support engineers.
    -   No: Go to the next step.
2.  Run the `GET /_cat/indices?v` command to check whether multiple system indexes whose names start with .security- exist in the cluster.
    -   Yes: You may have performed full data migration or synchronization. In this case, delete the .security-\* indexes of earlier versions.
    -   No: Submit a ticket to Alibaba Cloud Elasticsearch technical support engineers.

## Can I install custom plug-ins for Kibana?

No, Kibana versions earlier than V7.0 support only built-in plug-ins provided in the Kibana console, and Kibana V7.0 and later do not support plug-ins.

