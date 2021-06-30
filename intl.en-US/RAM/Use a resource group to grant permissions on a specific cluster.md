# Use a resource group to grant permissions on a specific cluster

This topic describes how to use a resource group to grant permissions on a specific cluster to a Resource Access Management \(RAM\) user in the RAM console.

## Background information

By default, Alibaba Cloud Elasticsearch clusters are created in the default resource group. After you attach a custom policy for a specific cluster to a RAM user and use the RAM user to log on to the Elasticsearch console, all the clusters of your Alibaba Cloud account rather than the specific cluster are displayed in the console. If you want the system to display only the specific cluster in the console, you can use a resource group to grant the permissions on the cluster to the RAM user.

## Step 1: Attach a custom policy whose effective scope is the entire Alibaba Cloud account to a RAM user of the account

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using an Alibaba Cloud account.

2.  Create a custom policy.

    1.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

    2.  On the **Policies** page, click **Create Policy**.

    3.  Enter a name in the **Policy Name** field.

    4.  Set **Configuration Mode** to **Script**.

        ![Create a custom policy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9219305261/p273771.png)

    5.  Set **Policy Document**. For more information, see the following example. Replace the Alibaba Cloud account ID and Elasticsearch cluster ID in the example with the ID of your Alibaba Cloud account and the ID of the desired Elasticsearch cluster.

        ```
        {
            "Statement": [
                {
                    "Action": [
                        "elasticsearch:*"
                    ],
                    "Effect": "Allow",
                    "Resource": "acs:elasticsearch:*:133071096032****:instances/es-cn-tl325goel000j****"
                },
                
                    "Action": [
                        "elasticsearch:ListCollectors"
                    ],
                    "Effect": "Allow",
                    "Resource": "acs:elasticsearch:*:133071096032****:collectors/*"
                },
                {
                    "Effect": "Allow",
                    "Action": [
                        "cms:ListAlarm",
                        "cms:DescribeActiveMetricRuleList",
                        "cms:QueryMetricList"
                    ],
                    "Resource": "*"
                },
                {
                    "Action": [
                        "elasticsearch:ListTags"
                    ],
                    "Effect": "Allow",
                    "Resource": "acs:elasticsearch:*:*:tags/*"
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

        External interfaces that are used to call some services, such as Beats, Advanced Monitoring and Alerting, and Tag, are integrated into the cluster management page of the Elasticsearch console. Therefore, if you want to manage only the clusters in a specific resource group in the console, you must configure a custom policy whose effective scope is the entire Alibaba Cloud account and attach the policy to the RAM user. This way, the RAM user can pass permission verification on the cluster management page.

    6.  Click **OK**.

3.  Create a RAM user.

    1.  In the left-side navigation pane, choose **Identities** \> **Users**.

    2.  Click **Create User**.

    3.  On the **Create User** page, set the **Logon Name** and **Display Name** parameters.

        ![Create a RAM user](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9219305261/p274500.png)

    4.  Click **OK**. The created RAM user appears on the Users page.

        ![Created RAM user](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9219305261/p274245.png)

4.  Attach the created custom policy whose effective scope is the entire Alibaba Cloud account to the RAM user.

    1.  Find the RAM user on the **Users** page.

    2.  Click **Add Permissions** in the **Actions** column that corresponds to the RAM user.

    3.  In the **Add Permissions** panel, click **Custom Policy** in the Select Policy section and click the name of the newly created custom policy in the Authorization Policy Name column.

        ![Attach the custom policy to the RAM user](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9219305261/p275907.png)

    4.  Click **OK**.

    5.  Click **Complete**.

        ![View the authorization result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9219305261/p276380.png)


## Step 2: Create a resource group and attach a policy to the resource group

1.  Log on to the [Resource Management console](https://resourcemanager.console.aliyun.com/resource-groups).

2.  Create a resource group.

    1.  In the left-side navigation pane, click **Resource Group**.

    2.  On the Resource Group page, click **Create Resource Group**.

        ![Create a resource group](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9219305261/p274517.png)

    3.  In the **Create Resource Group** panel, set the **Resource Group Name** and **Display Name** parameters.

    4.  Click **OK**.

3.  Move the desired cluster in the default resource group to the newly created resource group.

    1.  On the Resource Group page, click **Default Resource Group** in the Display Name column.

    2.  On the Default Resource Group page, click the **Resources** tab.

    3.  Select the desired cluster and click **Transfer Out** in the lower part of the page.

    4.  In the **Transfer Out** panel, select the newly created resource group.

    5.  Click **OK**.

4.  Attach a policy to the newly created resource group.

    1.  In the left-side navigation pane, click **Resource Group**.

    2.  Find the newly created resource group and click **Manage Permission** in the **Actions** column.

    3.  On the page that appears, click **Grant Permission**.

    4.  In the **Grant Permission** panel, set the parameters.

        ![Grant Permission panel](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1683405261/p274758.png)

    5.  Click **OK**.

    6.  Click **Complete**.

5.  View the authorization information of the RAM user.

    1.  Click the **Permissions** tab.

    2.  Click the name of the RAM user in the **Principal** column.

        ![Principal](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9219305261/p274774.png)

    3.  On the page that appears, click the **Permissions** tab and view the authorization information of the RAM user.

        ![Basic user information](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0319305261/p275237.png)


## Step 3: Log on to the Elasticsearch console by using the RAM user

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home) by using the RAM user.

2.  In the top navigation bar, select the region where the desired cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**.

4.  In the top navigation bar, select the newly created resource group and view the information of the cluster.


