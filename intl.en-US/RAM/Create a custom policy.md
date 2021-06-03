---
keyword: Elasticsearch custom policy
---

# Create a custom policy

If the system policies provided by Alibaba Cloud Elasticsearch do not meet your requirements, you can create custom policies. Custom policies enable finer-grained permission management than system policies. This topic describes how to create a custom policy and provides policy examples.

You have understood the policy structure and syntax. For more information, see [Policy structure and syntax](/intl.en-US/Policy Management/Policy language/Policy structure and syntax.md).

Elasticsearch supports the following system policies:

-   AliyunElasticsearchReadOnlyAccess: the read-only permissions on Elasticsearch or Logstash clusters. This policy can be attached to read-only users.
-   AliyunElasticsearchFullAccess: the management permissions on Elasticsearch or Logstash clusters. This policy can be attached to administrators.

**Note:**

-   You can use the preceding policies to grant the permissions only on Elasticsearch or Logstash clusters to RAM users. The policies do not contain permissions on CloudMonitor and tags. If you want to grant the permissions on CloudMonitor or tags, you must create the related custom policies and attach the policies to the RAM users. For more information about how to grant the permissions on CloudMonitor or tags, see [Policy for an administrator](#section_q2g_cas_kj0).
-   You can use policies to grant permissions only on all the resources that belong to an Alibaba Cloud account. You cannot use the policies to grant permissions on a specific resource group.

## Procedure

1.  On the Policies page, click **Create Policy**.

2.  In the **Policy Document** section, select an existing system policy and modify the policy document.

    ![Policy Document section](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5750988061/p96968.png)

    **Note:** You can enter a keyword in the search box to perform a fuzzy search.

    Enter a permission script based on your business requirements. Examples:

    -   Permission to access the virtual private clouds \(VPCs\) that belong to your Alibaba Cloud account

        ```
        "elasticsearch:DescribeVpcs","elasticsearch:DescribeVswitches"
        ```

        **Note:** For more information about the related policy document, see the document of the **AliyunVPCReadOnlyAccess** policy.

    -   Permission to pay for orders

        ```
        ["bss:PayOrder"] 
        ```

        **Note:** For more information about the related policy document, see the document of the **AliyunBSSOrderAccess** policy.

    -   Permission to call API operations

        |Method|URI|Resource|Action|
        |------|---|--------|------|
        |GET|/instances|instances/\*|ListInstance|
        |POST|/instances|instances/\*|CreateInstance|
        |GET|/instances/$instanceId|instances/$instanceId|DescribeInstance|
        |DELETE|/instances/$instanceId|instances/$instanceId|DeleteInstance|
        |POST|/instances/$instanceId/actions/restart|instances/$instanceId|RestartInstance|
        |PUT|/instances/$instanceId|instances/$instanceId|UpdateInstance|

    For more information about policy examples, see [Policy examples](#section_q2g_cas_kj0).

3.  Click **OK**.


## Policy examples

-   Policy for an administrator

    In this example, all the operation permissions on all the Elasticsearch clusters are granted to a RAM user of the Alibaba Cloud account whose ID is indicated by <UID\>.

    ```
    {
        "Statement": [
            {
                "Action": [
                    "elasticsearch:*"
                ],
                "Effect": "Allow",
                "Resource": "*"
            },
            {
                "Action": [
                    "cms:*"
                ],
                "Effect": "Allow",
                "Resource": "*"
            },
            {
                "Action": "bss:PayOrder",
                "Effect": "Allow",
                "Resource": "*"
            },
            {
                "Action": "ram:CreateServiceLinkedRole",
                "Resource": "*",
                "Effect": "Allow",
                "Condition": {
                    "StringEquals": {
                        "ram:ServiceName": [
                            "collector.elasticsearch.aliyuncs.com",
                            "ops.elasticsearch.aliyuncs.com"
                        ]
                    }
                }
            }
        ],
        "Version": "1"
    }
    ```

    **Note:**

-   Policy for cluster-specific operation permissions

    In this example, the following permissions are granted to a RAM user of the Alibaba Cloud account whose ID is indicated by <UID\>:

    -   [Permissions on CloudMonitor](/intl.en-US/RAM/Types of resources that can be authorized.md)
    -   Permission to perform all Elasticsearch-related operations on a specific cluster. The operations do not include security-related operations, such as cluster deletion and whitelist configuration for access over the Internet or an internal network.
    -   Permission to view clusters.
    -   Permission to view all the tags that are added to clusters.
    -   Permission to view shippers.
    ```
    {
        "Statement": [
            {
                "Action": [
                    "elasticsearch:*"
                ],
                "Effect": "Allow",
                "Resource": "acs:elasticsearch:*:<UID\>:instances/<instanceId\>"
            },
            {
                "Action": [
                    "cms:ListProductOfActiveAlert",
                    "cms:ListAlarm",
                    "cms:QueryMetricList"
                ],
                "Effect": "Allow",
                "Resource": "*"
            },
            {
                "Action": [
                    "elasticsearch:DeleteInstance",
                    "elasticsearch:UpdatePublicNetwork",
                    "elasticsearch:UpdateWhiteIps",
                    "elasticsearch:ModifyWhiteIps",
                    "elasticsearch:TriggerNetwork"
                ],
                "Effect": "Deny",
                "Resource": "acs:elasticsearch:*:<UID\>:instances/*"
            },
            {
                "Action": [
                    "elasticsearch:ListTags"
                ],
                "Effect": "Allow",
                "Resource": "acs:elasticsearch:*:<UID\>:tags/*"
            },
            {
                "Action": [
                    "elasticsearch:ListInstance",
                    "elasticsearch:ListSnapshotReposByInstanceId"
                ],
                "Effect": "Allow",
                "Resource": "acs:elasticsearch:*:<UID\>:instances/*"
            },
            {
                "Action": [
                    "elasticsearch:ListCollectors"
                ],
                "Effect": "Allow",
                "Resource": "acs:elasticsearch:*:<UID\>:collectors/*"
            },
            {
                "Action": [
                    "elasticsearch:ListLogstash"
                ],
                "Effect": "Allow",
                "Resource": "acs:elasticsearch:*:<UID\>:logstashes/*"
            },
            {
                "Action": [
                    "elasticsearch:GetEmonProjectList"
                ],
                "Effect": "Allow",
                "Resource": "acs:elasticsearch:*:*:emonProjects/*"
            },
            {
                "Action": [
                    "elasticsearch:getEmonUserConfig"
                ],
                "Effect": "Allow",
                "Resource": "acs:elasticsearch:*:*:emonUserConfig/*"
            }
        ],
        "Version": "1"
    }
    ```


|Action|Description|
|------|-----------|
|```
[
  "cms:ListProductOfActiveAlert",
  "cms:ListAlarm",
  "cms:QueryMetricList"
]
```

|The permissions on CloudMonitor.-   `cms:ListProductOfActiveAlert`: the permission to query the services for which CloudMonitor is activated within the Alibaba Cloud account.
-   `cms:ListAlarm`: the permission to query all or specific alert rules.
-   `cms:QueryMetricList`: the permission to query the monitoring data of instances or clusters of a specific service within a period. |
|```
[
  "bss:PayOrder"
]
```

|The permission to pay for orders. After the RAM user is granted the permission, the RAM user can pay for purchase orders of resources.|
|```
[
  "elasticsearch:DescribeVpcs",
  "elasticsearch:DescribeVswitches"
]
```

|The permissions to access the VPCs and vSwitches that belong to the Alibaba Cloud account. After the RAM user is granted the permissions, the RAM user can select the VPC and vSwitch that belong to the Alibaba Cloud account when the user purchases resources. **Note:** When you authorize a RAM user to purchase resources, you must also specify `["bss:PayOrder"]` in the Action element. If you do not specify \["bss:PayOrder"\], the system displays a message that indicates insufficient permissions when you use the RAM user to purchase resources. |
|```
[
  "elasticsearch:*"
]
```

|All operation permissions on Elasticsearch clusters. After the RAM user is granted the permissions, the RAM user can perform operations on all or specific clusters. **Note:** The permissions specified by `elasticsearch:*` do not include the permissions on advanced monitoring and alerting, CloudMonitor, and tags. You must separately specify the permissions on advanced monitoring and alerting, CloudMonitor, and tags. If you do not specify these permissions, the system displays a message that indicates insufficient permissions after you use the RAM user to go to a related page. However, the RAM user can use other authorized features on this page. |
|```
[
  "elasticsearch:ListTags"
]
```

|The permission to query all the tags that are added to Elasticsearch clusters. After the RAM user is granted the permission, the RAM user can view all the tags that are added to Elasticsearch clusters.|
|```
[
  "elasticsearch:ListInstance",
  "elasticsearch:ListSnapshotReposByInstanceId" 
]
```

|-   `elasticsearch:ListInstance`: the permission to query Elasticsearch clusters.
-   `elasticsearch:ListSnapshotReposByInstanceId`: the permission to query shared Object Storage Service \(OSS\) repositories. |
|```
[
  "elasticsearch:ListCollectors"
]
```

|The permission to query Beats shippers. After the RAM user is granted the permission, the RAM user can view all the created Beats shippers in the Elasticsearch console.|
|```
[
  "elasticsearch:ListLogstash"
]
```

|The permission to query Logstash clusters. After the RAM user is granted the permission, the RAM user can view all the Logstash clusters in the related region on the Logstash Clusters page.|
|```
[
  "elasticsearch:DeleteInstance",
  "elasticsearch:UpdatePublicNetwork",
  "elasticsearch:TriggerNetwork"
]
```

|-   `elasticsearch:DeleteInstance`: the permission to delete clusters.
-   `elasticsearch:UpdatePublicNetwork`: the permission to modify the whitelist for access to clusters over the Internet.
-   `elasticsearch:TriggerNetwork`: the permission to enable or disable the Public Network Access or Private Network Access feature for Elasticsearch or Kibana.

**Note:** In the preceding policy, the Effect element is set to Deny for these permissions. This indicates that the RAM user is not allowed to perform the related operations. |
|```
[
  "elasticsearch:GetEmonProjectList"
]
```

|The permission to query the monitoring items of a cluster. **Note:** If the RAM user is granted the permission, the `["elasticsearch:getEmonUserConfig"]` permission must also be granted. If \["elasticsearch:getEmonUserConfig"\] is not granted, the system displays a message that indicates insufficient permissions when you use the RAM user to go to the cluster monitoring page. |
|```
[
  "elasticsearch:getEmonUserConfig"
]
```

|The permission to query the user configurations for cluster monitoring.|

|Effect|Description|
|------|-----------|
|Allow|Indicates that the RAM user is allowed to perform the operations that are specified in the Action element.|
|Deny|Indicates that the RAM user is not allowed to perform the operations that are specified in the Action element.|

**Note:**

-   <UID\>: Set the value to the ID of your Alibaba Cloud account. To obtain the ID, move the pointer over the profile picture in the upper-right corner of the Elasticsearch console and click **Security Settings**. The value of **Account ID** on the **Security Settings** page is the ID of your Alibaba Cloud account.
-   <instanceId\>: Set the value to the ID of the cluster whose permissions you want to grant. For more information about how to obtain the ID, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).
-   In the Resource element, an asterisk \(`*`\) indicates all clusters, and <instanceId\> indicates a specific cluster. For more information, see [Types of resources that can be authorized](/intl.en-US/RAM/Types of resources that can be authorized.md).

After a custom policy is created, use your Alibaba Cloud account to attach the policy to a RAM user in the RAM console or by using a RAM SDK. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM/Grant permissions to a RAM user.md).

