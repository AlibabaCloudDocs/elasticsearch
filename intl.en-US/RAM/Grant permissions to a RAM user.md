---
keyword: grant permissions to a RAM user
---

# Grant permissions to a RAM user

This topic describes how to grant a RAM user operation permissions on Elasticsearch, such as the cluster creation and query permissions. Elasticsearch supports system and custom policies.

-   A RAM user is created. For more information, see [Create a RAM user](/intl.en-US/RAM User Management/Basic operations/Create a RAM user.md).
-   A custom policy is created.

    If system policies do not meet your requirements, create a custom policy. For more information, see [Create a custom policy](/intl.en-US/RAM/Create a custom policy.md).

    **Note:** Alibaba Cloud Elasticsearch supports the following system policies:

    -   `AliyunElasticsearchReadOnlyAccess`: grants the read-only permissions on Elasticsearch to users.
    -   `AliyunElasticsearchFullAccess`: grants the management permissions on Elasticsearch to the administrator.

This topic describes how to grant permissions to a RAM user on the Grants page of the RAM console. You can also grant permissions to a RAM user on the Users page. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Authorization management/Grant permissions to a RAM user.md).

**Note:** The cloud account in this topic is Alibaba Cloud accounts.

1.  In the left-side navigation pane, click **Grants** under **Permissions**.

2.  Click **Grant Permission**.

3.  Under **Principal**, enter a principal name and click the target principal.

    **Note:** You can enter a name of the RAM user, user group, or role for a fuzzy search.

4.  In the **Select Policy** section, grant permissions to the principal.

    1.  Select **System Policy** or **Custom Policy**.

        ![Add general policies](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3724309951/p96955.png)

    2.  Find the policy that you want to attach to the RAM user.

        **Note:** You can also enter the policy name in the search box to perform fuzzy match.

    3.  In the **Authorization Policy Name** column, click the required policy.

5.  Click **OK**.

6.  Click **OK**.

    **Note:** If a RAM user no longer requires a permission, you can remove the permission for the RAM user. For more information, see [Remove permissions from a RAM user](/intl.en-US/RAM User Management/Authorization management/Remove permissions from a RAM user.md).


