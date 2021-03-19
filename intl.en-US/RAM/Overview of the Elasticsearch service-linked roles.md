# Overview of the Elasticsearch service-linked roles

The Elasticsearch service-linked roles AliyunServiceRoleForElasticsearchOps and AliyunServiceRoleForElasticsearchCollector are RAM roles. They are used to implement auto scaling for Elasticsearch clusters, create and manage Beats shippers, and grant access permissions on other Alibaba Cloud services. This topic describes the use scenarios of the service-linked roles and how to delete the roles.

## Background information

For more information about the service-linked roles, see [Service-linked roles](/intl.en-US/RAM Role Management/Service-linked roles.md).

## Scenarios

The AliyunServiceRoleForElasticsearchOps and AliyunServiceRoleForElasticsearchCollector roles are used in the following scenarios:

-   AliyunServiceRoleForElasticsearchOps

    If you [scale out or in an Elasticsearch cluster](), you must use the service-linked role to authorize the system to call the related API operation. This way, the system can perform auto scaling for the cluster based on the time that you specified.

-   AliyunServiceRoleForElasticsearchCollector

    When you [create and manage a Beats shipper](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md), you must use the service-linked role to authorize the shipper to perform specific operations on an Elastic Compute Service \(ECS\) instance or a Container Service for Kubernetes \(ACK\) cluster.


## Overview of AliyunServiceRoleForElasticsearchOps

Elasticsearch can perform auto scaling for clusters only after it assumes a role that has the required permissions. If such a role does not exist, Elasticsearch automatically creates the service-linked role AliyunServiceRoleForElasticsearchOps and grants the required permissions to the role during auto scaling. Elasticsearch assumes the role to call the related API operation and perform auto scaling for your cluster based on the time that you specified. The following descriptions provide detailed information about the role:

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

-   Service name: ops.elasticsearch.aliyuncs.com
-   Permission required to create the service-linked role: ram:CreateServiceLinkedRole

## Overview of AliyunServiceRoleForElasticsearchCollector

Elasticsearch can create and manage a Beats shipper only after it assumes a role that has the required permissions. If such a role does not exist, Elasticsearch automatically creates the service-linked role AliyunServiceRoleForElasticsearchCollector and grants the required permissions to the role. Elasticsearch assumes the role to call the related API operation and enables the Beats shipper to collect data from an ECS instance or an ACK cluster. The following descriptions provide detailed information about the role:

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

## Delete the service-linked roles

Before you delete the AliyunServiceRoleForElasticsearchOps service-linked role, you must stop the auto scaling task that depends on the role. Before you delete the AliyunServiceRoleForElasticsearchCollector service-linked role, you must delete all Beats shippers that depend on the role.

For more information about how to delete a service-linked role, see [Delete a service-linked role](/intl.en-US/RAM Role Management/Service-linked roles.md).

## FAQ

Q: Why am I unable to use my RAM user to create the Elasticsearch service-linked roles?

A: Only Alibaba Cloud accounts and RAM users that have the CreateServiceLinkedRole permission can be used to create or delete a service-linked role. Therefore, if your RAM user cannot be used to create the service-linked roles, you must attach the following policies to your RAM user.

**Note:**

-   For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).
-   You must replace the value of AccountId in the following policies with the ID of your Alibaba Cloud account.

-   AliyunServiceRoleForElasticsearchOps

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

-   AliyunServiceRoleForElasticsearchCollector

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


