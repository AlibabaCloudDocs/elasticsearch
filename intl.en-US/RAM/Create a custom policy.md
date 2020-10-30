---
keyword: Elasticsearch custom policy
---

# Create a custom policy

This topic describes how to create a custom policy in Elasticsearch. Custom policies enable finer-grained access control than system policies.

You understand the policy structure and syntax. For more information, see [Policy structure and syntax](/intl.en-US/Policy Management/Policy language/Policy structure and syntax.md).

Alibaba Cloud Elasticsearch supports the following two types of system policies:

-   AliyunElasticsearchReadOnlyAccess: This policy grants read-only permissions to users.
-   AliyunElasticsearchFullAccess: This policy grants permissions to manage Alibaba Cloud Elasticsearch clusters and applies to the administrator.

**Note:**

-   The AliyunElasticsearchFullAccess and AliyunElasticsearchReadOnlyAccess policies grant the permissions on Alibaba Cloud Elasticsearch or Logstash to only RAM users. The permissions do not include permissions on Cloud Monitor and tags. If you want to grant the required permissions to the RAM users, attach the AliyunCloudMonitorFullAccess policy, the AliyunCloudMonitorReadOnlyAccess policy, [specific policies](/intl.en-US/Appendix 3 account authorization/Control permissions of RAM users.md) related to Cloud Monitor, or [custom policies related to tags](/intl.en-US/RAM/Grant permissions on tags to a RAM user.md) to the RAM users.
-   Policies are only used to grant permissions on all the resources under an Alibaba Cloud account. They cannot be used to grant permissions on a specific resource group.
-   
1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using an Alibaba Cloud account.

2.  In the left-side navigation pane, click **Policies** under **Permissions**.

3.  On the Policies page, click **Create Policy**.

4.  On the Create Custom Policy page, specify the **Policy Name** and **Note** parameters.

5.  Under **Configuration Mode**, select **Script**.

6.  In the **Policy Document** section, select an existing system policy and edit the script.

    ![Policy Document section](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1724309951/p96968.png)

    **Note:** You can enter keywords into the search box to perform a fuzzy search.

    Enter a permission script as required:

    -   Permission to access the virtual private cloud \(VPC\) to which the Elasticsearch cluster belongs

        ```
        "vpc:DescribeVSwitch*","vpc:DescribeVpc*"
        ```

        **Note:** For more information about **Policy Document**, see the template for the **AliyunVPCReadOnlyAccess** policy.

    -   Permission to purchase clusters

        ```
        ["bss:PayOrder"] 
        ```

        **Note:** For more information about **Policy Document**, see the template for the **AliyunBSSOrderAccess** policy.

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

        **Note:** If tags are bound to your Elasticsearch cluster, you must grant operation permissions on the tags to RAM users. For more information, see [Grant permissions on tags to a RAM user](/intl.en-US/RAM/Grant permissions on tags to a RAM user.md).

        -   In the following example, a RAM user of the Alibaba Cloud account whose account ID is 1234 is granted all operation permissions other than cluster creation in the China \(Hangzhou\) region. The RAM user is also authorized to access all Elasticsearch clusters only by using specified IP addresses.

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

        -   In the following example, a RAM user of the Alibaba Cloud account whose account ID is 1234 is granted all operation permissions other than cluster creation in the China \(Hangzhou\) region. The RAM user is also authorized to access specific Elasticsearch clusters only by using specified IP addresses.

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

        -   In the following example, a RAM user of the Alibaba Cloud account whose account ID is 1234 is granted all operation permissions on all Elasticsearch clusters in all regions.

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

        -   In the following example, a RAM user of the Alibaba Cloud account whose account ID is 1234 is granted all operation permissions other than cluster creation and query in all regions.

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

7.  Click **OK**.


You can use the created custom policy to grant permissions to RAM users. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM/Grant permissions to a RAM user.md).

