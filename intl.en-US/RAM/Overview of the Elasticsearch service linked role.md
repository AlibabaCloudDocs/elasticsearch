# Overview of the Elasticsearch service linked role

This topic describes the use scenarios of the service linked role for Alibaba Cloud Elasticsearch and how to delete the role. The role is named AliyunServiceRoleForElasticsearchOps.

## Background information

The Elasticsearch service linked role AliyunServiceRoleForElasticsearchOps is a RAM role. It is used to grant access permissions on other Alibaba Cloud services when you use an Elasticsearch feature. For more information about the service linked role, see [Service-linked roles](/intl.en-US/RAM Role Management/Service-linked roles.md).

## Scenarios

If you [scale out or in an Elasticsearch cluster](), you must use the service linked role to authorize the system to call the related API operation. This way, the system can perform auto scaling for the cluster based on the time you specified.

## Overview

Elasticsearch can perform auto scaling for clusters only after it assumes a role that has the required permissions. If no such a role exists, Elasticsearch automatically creates the service linked role AliyunServiceRoleForElasticsearchOps and grants the required permissions to the role during auto scaling. Elasticsearch assumes the role to call the API operation and perform auto scaling for your cluster based on the time that you specified. The following content provides detailed information about the role:

-   Role name: AliyunServiceRoleForElasticsearchOps
-   Name of the permission policy for the role: AliyunServiceRolePolicyForElasticsearchOps
-   Policy document:

    ```
    {
      "Version": "1",
      "Statement": [
        {
          "Action": [
            "elasticsearch:ListInstance",
            "elasticsearch:DescribeInstance",
            "elasticsearch:UpdateInstance",
            "elasticsearch:UpdateInstanceSettings",
            "elasticsearch:RestartInstance",
            "elasticsearch:RollbackInstance",
            "elasticsearch:DowngradeInstance",
            "elasticsearch:CancelTask",
            "elasticsearch:DeactivateZones",
            "elasticsearch:ActivateZones",
            "elasticsearch:MigrateToOtherZone",
            "elasticsearch:ResumeElasticsearchTask",
            "elasticsearch:InterruptElasticsearchTask",
            "elasticsearch:UpdateAdvancedSetting",
            "elasticsearch:UpgradeInstanceEngineVersion",
            "elasticsearch:UpdateWhiteIps",
            "elasticsearch:UpdatePublicIps",
            "elasticsearch:ModifyWhiteIps",
            "elasticsearch:TriggerNetwork",
            "elasticsearch:UpdateTemplate",
            "elasticsearch:DescribeLogstash",
            "elasticsearch:UpdateLogstash",
            "elasticsearch:RestartLogstash",
            "elasticsearch:UpdateLogstashSettings",
            "elasticsearch:InterruptLogstashTask",
            "elasticsearch:ResumeLogstashTask",
            "elasticsearch:DowngradeLogstash"
          ],
          "Resource": "*",
          "Effect": "Allow"
        }
      ]
    }
    ```

    **Note:** The Elasticsearch service linked role is used only in auto scaling scenarios. In the future, it will also be used in more operations and maintenance \(O&M\) scenarios.

-   Service name: ops.elasticsearch.aliyuncs.com
-   Permission required to create the service linked role: ram:CreateServiceLinkedRole.

## Delete the service linked role

Before you delete the service linked role, you must stop the auto scaling task that depends on the role.

For more information about how to delete a service linked role, see [Delete a service-linked role](/intl.en-US/RAM Role Management/Service-linked roles.md).

## FAQ

Q: Why am I unable to use my RAM user to create the service linked role AliyunServiceRoleForElasticsearchOps?

A: Only Alibaba Cloud accounts and RAM users that have the CreateServiceLinkedRole permission can be used to create or delete a service linked role. Therefore, if your RAM user cannot be used to create AliyunServiceRoleForElasticsearchOps, you must attach the following policy to your RAM user by using your Alibaba Cloud account. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).

```
{
    "Statement": [
        {
            "Action": [
                "ram:CreateServiceLinkedRole"
            ],
            "Resource": "acs:ram:*:${AccountId}:role/*",
            "Effect": "Allow",
            "Condition": {
                "StringEquals": {
                    "ram:ServiceName": [
                        "ops.elasticsearch.aliyuncs.com"
                    ]
                }
            }
        }
    ],
    "Version": "1"
}
```

**Note:** Replace `AccountId` with the ID of your Alibaba Cloud account.

