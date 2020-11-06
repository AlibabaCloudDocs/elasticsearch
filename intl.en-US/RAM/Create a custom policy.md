---
keyword: Elasticsearch custom policy
---

# Create a custom policy

This topic describes how to create a custom policy in Alibaba Cloud Elasticsearch. Custom policies enable finer-grained access control than system policies.

You have understood the policy structure and syntax. For more information, see [Policy structure and syntax](/intl.en-US/Policy Management/Policy language/Policy structure and syntax.md).

Elasticsearch supports the following system policies:

-   AliyunElasticsearchReadOnlyAccess: grants the read-only permissions on Elasticsearch clusters to users.
-   AliyunElasticsearchFullAccess: grants the management permissions on Elasticsearch clusters to the administrator.

**Note:**

-   The AliyunElasticsearchFullAccess and AliyunElasticsearchReadOnlyAccess policies grant the permissions on Elasticsearch to only RAM users. The permissions do not include permissions on Cloud Monitor and tags. If you want to grant the permissions on Cloud Monitor or tags, attach AliyunCloudMonitorFullAccess, AliyunCloudMonitorReadOnlyAccess, [specific policies related to Cloud Monitor](/intl.en-US/Appendix 3 account authorization/Control permissions of RAM users.md), or [tag-related custom policies](/intl.en-US/RAM/Grant permissions on tags to a RAM user.md) to the users.
-   You can use policies to grant permissions only on all the resources under an Alibaba Cloud account. You cannot use them to grant permissions on a specific resource group.
-   
1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using an Alibaba Cloud account.

2.  In the left-side navigation pane, click **Policies** under **Permissions**.

3.  On the page that appears, click **Create Policy**.

4.  On the Create Custom Policy page, specify the **Policy Name** and **Note** parameters.

5.  Under **Configuration Mode**, select **Script**.

6.  In the **Policy Document** section, select an existing system policy and edit the policy document.

    ![Policy Document section](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1724309951/p96968.png)

    **Note:** You can enter a keyword into the search box to perform a fuzzy search.

    Enter a permission script as required.

    -   Permission to access the virtual private cloud \(VPC\) to which your Elasticsearch cluster belongs

        ```
        "vpc:DescribeVSwitch*","vpc:DescribeVpc*"
        ```

        **Note:** For more information about the related **policy document**, see the template for the **AliyunVPCReadOnlyAccess** policy.

    -   Permission to purchase clusters

        ```
        ["bss:PayOrder"] 
        ```

        **Note:** For more information about the related **policy document**, see the template for the **AliyunBSSOrderAccess** policy.

    -   Permission to call API operations

        |Method|URI|Resource|Action|
        |------|---|--------|------|
        |GET|/instances|instances/\*|ListInstance|
        |POST|/instances|instances/\*|CreateInstance|
        |GET|/instances/$instanceId|instances/$instanceId|DescribeInstance|
        |DELETE|/instances/$instanceId|instances/$instanceId|DeleteInstance|
        |POST|/instances/$instanceId/actions/restart|instances/$instanceId|RestartInstance|
        |PUT|/instances/$instanceId|instances/$instanceId|UpdateInstance|

        Examples:

        **Note:** If tags are added to your Elasticsearch cluster, you must grant operation permissions on the tags to RAM users. For more information, see [Grant permissions on tags to a RAM user](/intl.en-US/RAM/Grant permissions on tags to a RAM user.md).

        -   In the following example, a RAM user of the Alibaba Cloud account whose ID is 1234 is granted all operation permissions other than cluster creation in the China \(Hangzhou\) region. The RAM user is also authorized to access all Elasticsearch clusters in this region by using only the specified IP addresses.

            ```
            {
              "Statement": [
                {
                  "Action": [
                    "elasticsearch:ListInstance",
                    "elasticsearch:DescribeInstance",
                    "elasticsearch:DeleteInstance",
                    "elasticsearch:RestartInstance",
                    "elasticsearch:UpdateInstance"
                  ],
                  "Condition": {
                    "IpAddress": {
                      "acs:SourceIp": "xxx.xx.xxx.x/xx"
                    }
                  },
                  "Effect": "Allow",
                  "Resource": "acs:elasticsearch:cn-hangzhou:1234:instances/*"
                }
              ],
              "Version": "1"
            }
            ```

        -   In the following example, a RAM user of the Alibaba Cloud account whose ID is 1234 is granted all operation permissions other than cluster creation in the China \(Hangzhou\) region. The RAM user is also authorized to access specific Elasticsearch clusters in this region by using only the specified IP addresses.

            ```
            {
              "Statement": [
                {
                  "Action": [
                    "elasticsearch:ListInstance"
                  ],
                  "Condition": {
                    "IpAddress": {
                      "acs:SourceIp": "xxx.xx.xxx.x/xx"
                    }
                  },
                  "Effect": "Allow",
                  "Resource": "acs:elasticsearch:cn-hangzhou:1234:instances/*"
                },
                {
                  "Action": [
                    "elasticsearch:DescribeInstance",
                    "elasticsearch:DeleteInstance",
                    "elasticsearch:RestartInstance",
                    "elasticsearch:UpdateInstance"
                  ],
                  "Condition": {
                    "IpAddress": {
                      "acs:SourceIp": "xxx.xx.xxx.x/xx"
                    }
                  },
                  "Effect": "Allow",
                  "Resource": "acs:elasticsearch:cn-hangzhou:1234:instances/$instanceId"
                }
              ],
              "Version": "1"
            }
            ```

        -   In the following example, a RAM user of the Alibaba Cloud account whose ID is 1234 is granted all operation permissions in all regions.

            ```
            {
              "Statement": [
                {
                  "Action": [
                      "elasticsearch:*"
                        ],
                  "Effect": "Allow",
                  "Resource": "acs:elasticsearch:*:1234:instances/*"
                }
              ],
              "Version": "1"
            }
            ```

        -   In the following example, a RAM user of the Alibaba Cloud account whose ID is 1234 is granted all operation permissions other than cluster creation and query in all regions.

            ```
            {
              "Statement": [
                {
                  "Action": [
                      "elasticsearch:DescribeInstance",
                      "elasticsearch:DeleteInstance",
                      "elasticsearch:UpdateInstance",
                      "elasticsearch:RestartInstance"
                        ],
                  "Effect": "Allow",
                  "Resource": "acs:elasticsearch:*:1234:instances/$instanceId"
                }
              ],
              "Version": "1"
            }
            ```

    -   In the following example, a RAM user of the Alibaba Cloud account whose ID is 1234 is granted some permissions in the China \(Hangzhou\) region. The RAM user is authorized to perform the following operations in the region:

        -   Configure the public IP address whitelists for all Elasticsearch clusters or the Kibana services of these clusters.
        -   Query shippers.
        -   Query the tags that are added to all Elasticsearch clusters.
        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Action": [
                        "elasticsearch:UpdateKibanaIps",
                        "elasticsearch:ListInstance",
                        "elasticsearch:DescribeInstance",
                        "elasticsearch:DescribeConnectableClusters",
                        "elasticsearch:ListConnectedClusters",
                        "elasticsearch:DescribeElasticsearchHealth",
                        "elasticsearch:DescribeKibanaSettings",
                        "elasticsearch:ListKibanaPlugins",
                        "elasticsearch:ModifyWhiteIps"
                    ],
                    "Effect": "Allow",
                    "Resource": "acs:elasticsearch:cn-hangzhou:1234:instances/*"
                },
                {
                    "Action": [
                        "elasticsearch:ListCollectors"
                    ],
                    "Effect": "Allow",
                    "Resource": "acs:elasticsearch:cn-hangzhou:1234:collectors/*"
                },
                {
                    "Action": [
                        "elasticsearch:ListTags"
                    ],
                    "Effect": "Allow",
                    "Resource": "acs:elasticsearch:cn-hangzhou:1234:tags/*"
                }
        
            ]
        }
        ```

        **Note:**

        -   If you specify the ModifyWhiteIps operation in a policy, you must also specify other console-related operations, as shown in the preceding policy.
        -   If you specify a whitelist update-related operation such as UpdatePublicIps, UpdateWhiteIps, or UpdateKibanaIps for the Action parameter in a policy, you must also specify ModifyWhiteIps.
7.  Click **OK**.


Use the custom policies you created to grant permissions to RAM users. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM/Grant permissions to a RAM user.md).

