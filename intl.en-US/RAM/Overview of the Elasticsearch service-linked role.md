# Overview of the Elasticsearch service-linked role

The Elasticsearch service-linked role AliyunServiceRoleForElasticsearchCollector is a Resource Access Management \(RAM\) role. It is used to create and manage Beats shippers and grant access permissions on other Alibaba Cloud services. This topic describes the use scenarios of the service-linked role and how to delete the role.

## Background information

For more information about the service-linked role, see [Service-linked roles](/intl.en-US/RAM Role Management/Service-linked roles.md).

## Scenarios

When you [create and manage a Beats shipper](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md), you must use the service-linked role AliyunServiceRoleForElasticsearchCollector to authorize the shipper to perform specific operations on an Elastic Compute Service \(ECS\) instance or Container Service for Kubernetes \(ACK\) cluster.

## Overview of AliyunServiceRoleForElasticsearchCollector

Elasticsearch can create and manage a Beats shipper only after it assumes a role that has the required permissions. If such a role does not exist, Elasticsearch automatically creates the service-linked role AliyunServiceRoleForElasticsearchCollector and grants the required permissions to the role. Elasticsearch assumes the role to call the related API operation and enables the Beats shipper to collect data from an ECS instance or ACK cluster. The following descriptions provide detailed information about the role:

-   Role name: AliyunServiceRoleForElasticsearchCollector
-   Name of the permission policy for the role: AliyunServiceRolePolicyForElasticsearchCollector
-   Policy document:

    ```
    {
      "Version": "1",
      "Statement": [
        {
          "Action": [
            "oos:CancelExecution",
            "oos:DeleteExecutions",
            "oos:GenerateExecutionPolicy",
            "oos:GetExecutionTemplate",
            "oos:ListExecutionLogs",
            "oos:ListExecutions",
            "oos:ListTaskExecutions",
            "oos:NotifyExecution",
            "oos:StartExecution",
            "oos:ListTagResources",
            "oos:TagResources",
            "oos:UntagResources",
            "oos:CreateTemplate",
            "oos:DeleteTemplate",
            "oos:GetTemplate",
            "oos:ListExecutionRiskyTasks",
            "oos:ListTemplates",
            "oos:UpdateTemplate"
          ],
          "Resource": "*",
          "Effect": "Allow"
        },
        {
          "Action": [
            "ecs:DescribeInstances",
            "ecs:DescribeCloudAssistantStatus"
          ],
          "Resource": "*",
          "Effect": "Allow"
        },
        {
          "Action": [
            "cs:GetUserConfig",
            "cs:GetClustersByUid",
            "cs:GetClusterInfo"
          ],
          "Resource": "*",
          "Effect": "Allow"
        },
        {
          "Action": "ram:DeleteServiceLinkedRole",
          "Resource": "*",
          "Effect": "Allow",
          "Condition": {
            "StringEquals": {
              "ram:ServiceName": "collector.elasticsearch.aliyuncs.com"
            }
          }
        },
        {
          "Effect": "Allow",
          "Action": "ram:PassRole",
          "Resource": "acs:ram:*:*:role/aliyunoosaccessingecs4esrole",
          "Condition": {
            "StringEquals": {
              "acs:Service": "oos.aliyuncs.com"
            }
          }
        }
      ]
    }
    ```

-   Service name: collector.elasticsearch.aliyuncs.com
-   Permission required to create or delete the service-linked role: ram:CreateServiceLinkedRole

## Delete the service-linked role

Before you delete the AliyunServiceRoleForElasticsearchCollector service-linked role, you must delete all the Beats shippers that depend on the role.

For more information about how to delete a service-linked role, see [Delete a service-linked role](/intl.en-US/RAM Role Management/Service-linked roles.md).

## FAQ

Q: Why am I unable to use my RAM user to create the Elasticsearch service-linked role?

A: Only Alibaba Cloud accounts and RAM users that have the CreateServiceLinkedRole permission can be used to create or delete a service-linked role. Therefore, if your RAM user cannot be used to create the service-linked role, you must attach the following policy to your RAM user.

**Note:**

-   For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).
-   You must replace AccountId in the following policy with the ID of your Alibaba Cloud account.

AliyunServiceRoleForElasticsearchCollector

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
                        "collector.elasticsearch.aliyuncs.com"
                    ]
                }
            }
        }
    ],
    "Version": "1"
}
```

