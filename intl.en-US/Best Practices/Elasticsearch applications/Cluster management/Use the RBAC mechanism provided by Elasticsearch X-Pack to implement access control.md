# Use the RBAC mechanism provided by Elasticsearch X-Pack to implement access control

If you want to configure access permissions on items such as clusters, indexes, and fields, you can use the role-based access control \(RBAC\) mechanism that is provided by the X-Pack plug-in of Elasticsearch. This mechanism allows you to grant permissions to custom roles and assign the roles to users to implement access control. Elasticsearch provides a variety of built-in roles. You can create custom roles based on the built-in roles to meet specific requirements. This topic describes how to create and configure a custom role to implement access control.

-   For more information about the RBAC mechanism, see [User authorization](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/authorization.html#roles).
-   Elasticsearch supports various security authentication features. For more information, see [Elasticsearch identity authentication and authorization](https://www.elastic.co/cn/blog/demystifying-authentication-and-authorization-in-elasticsearch).

## Procedure

1.  Create a role.

    For more information, see [Create a role](/intl.en-US/RAM/Manage Kibana role/Create a role.md). The following table describes the related parameters.

    ![Enter role information](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4511461161/p199132.png)

    |Parameter|Description|
    |---------|-----------|
    |**Role name**|The name of the role.|
    |**Run As privileges**|The user who assumes the role. This parameter is optional. If you do not specify this parameter, you can assign the role to a user when you create the user. For more information, see [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md).|
    |**Cluster privileges**|The operation permissions on the cluster, such as the permissions to view the cluster health status and settings and the permission to create snapshots. For more information, see [Cluster privileges](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-privileges.html#privileges-list-cluster).|
    |**Index privileges**|The operation permissions on indexes. For example, if you want to grant the role the read-only permissions on all fields in all indexes, set the Indices parameter to an asterisk \(\*\) and the Privileges parameter to read. You can set the Indices parameter to an [asterisk or regular expression](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/defining-roles.html#roles-indices-priv). For more information, see [Indices privileges](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-privileges.html#privileges-list-indices).|
    |**Kibana privileges**|The operation permissions on Kibana. **Note:** Versions earlier than Kibana V7.0 support only [base privileges](https://www.elastic.co/guide/en/kibana/6.7/kibana-privileges.html#kibana-privileges). Kibana V7.0 and later support base privileges and [feature privileges](https://www.elastic.co/guide/en/kibana/7.10/kibana-privileges.html#kibana-feature-privileges). After you assign a base privilege to a role, the role has the access permissions on all Kibana spaces. After you assign a feature privilege to a role, the role has the access permissions only on a specific feature. To assign a feature privilege, you must specify a Kibana space. |

    When you create a role, you must grant permissions to the role. In this example, the following permissions are granted:

    -   Read-only permissions on a specific index

        For more information, see [Configure read-only permissions on indexes](#section_y3r_vdm_0x4).

    -   Permissions to view all or some dashboards

        For more information, see [Configure operation permissions on dashboards](#section_gor_c8m_8cc).

    -   Read and write permissions on some indexes and read-only permissions on all clusters, such as the permissions to view cluster health statuses, snapshots, and settings, write data to indexes, or update index mappings

        For more information, see [Configure read and write permissions on indexes and read-only permissions on clusters](#section_b9o_76k_efl).

2.  Create a user and assign the role to the user.

    For more information, see [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md).

3.  Use the user to log on to the Kibana console and perform operations.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).


## Configure read-only permissions on indexes

-   Scenario

    Grant a common user the read-only permissions on a specific index. In this case, the user can query the index data in the Kibana console but cannot access clusters.

-   Role configuration

    ![Read-only permissions on a specific index](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4511461161/p199647.png)

    |Permission type|Permission key|Permission value|Description|
    |---------------|--------------|----------------|-----------|
    |**Index privileges**|**indices**|kibana\_sample\_data\_logs|The name of the index. You can specify a full index name, alias, wildcard, or regular expression. For more information, see [Indices Privileges](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/defining-roles.html#roles-indices-priv).|
    |**privileges**|read|The read-only permissions on the index. The read-only permissions include the permissions to call the count, explain, get, mget, scripts, search, and scroll APIs. For more information, see [privileges-list-indices](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-privileges.html#privileges-list-indices).|
    |**Granted fields \(optional\)**|\*|The index fields. The value \* indicates all fields.|
    |**Kibana privileges**|**privileges**|read|The read-only permissions on Kibana. The permissions are granted to all spaces. Default value: none. This value indicates that no spaces are authorized to access Kibana. **Note:** Versions earlier than Kibana V7.0 support only [base privileges](https://www.elastic.co/guide/en/kibana/6.7/kibana-privileges.html#kibana-privileges). Kibana V7.0 and later support base privileges and [feature privileges](https://www.elastic.co/guide/en/kibana/7.10/kibana-privileges.html#kibana-feature-privileges). After you assign a base privilege to a role, the role has the access permissions on all Kibana spaces. After you assign a feature privilege to a role, the role has the access permissions only on a specific feature. To assign a feature privilege, you must specify a Kibana space. |

-   Verification

    Use the common user to log on to the Kibana console and run an index read command. The system returns results as expected. Then, run an index write command. The system returns an error message. The message indicates that the user is not authorized to perform write operations.

    ```
    GET /kibana_sample_data_logs/_search
    ```

    ```
    POST /kibana_sample_data_logs/_doc/1
    {
        "productName": "testpro",
        "annual_rate": "3.22%",
        "describe": "testpro"
    }
    ```

    ![Verify the read-only permissions](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4511461161/p200448.png)


## Configure operation permissions on dashboards

-   Scenario

    Grant a common user the read-only permissions on a specific index and the permissions to view the dashboards for the index.

-   Role configuration

    When you [create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md), assign the read-index and kibana\_dashboard\_only\_user roles to the user.

    ![Role configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4511461161/p199843.png)

    -   read-index: a custom role. You must manually create a custom role. This role has read-only permissions on the specific index.
    -   kibana\_dashboard\_only\_user: a Kibana built-in role. This role has the permissions to view the dashboards for the index.

        **Note:**

        -   In Kibana V7.0 and later, the kibana\_dashboard\_only\_user role is deprecated. If you want to view the dashboards for a specific index, you can configure only the read-only permissions on the index. For more information, see [Configure read-only permissions on indexes](#section_y3r_vdm_0x4).
        -   The kibana\_dashboard\_only\_user role can be used with custom roles in various scenarios. If you want to configure the **Dashboards only roles** feature only for a custom role, perform the following steps: In the **Kibana** section of the **Management** page, click **Advanced Settings**. Then, in the **Dashboard** section on the page that appears, set the Dashboards only roles parameter to the custom role. The default value of this parameter is kibana\_dashboard\_only\_user.
-   Verification

    Use the common user to log on to the Kibana console and view the dashboards for the specific index.

    ![View dashboards](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4511461161/p199757.png)


## Configure read and write permissions on indexes and read-only permissions on clusters

-   Scenario

    Grant a common user the read, write, and delete permissions on specific indexes and the read-only permissions on clusters and Kibana.

-   Role configuration

    ![Read and write permissions on indexes and read-only permissions on clusters](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4511461161/p199616.png)

    |Permission type|Permission key|Permission value|Description|
    |---------------|--------------|----------------|-----------|
    |**Cluster privileges**|**cluster**|monitor|The read-only permissions on clusters, such as the permissions to view the running statuses, health statuses, hot threads, node information, and blocked tasks of clusters.|
    |**Index privileges**|indices|heartbeat-\*,library\*|The name of the index. You can specify a full index name, alias, wildcard, or regular expression. For more information, see [roles-indices-privileges](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/defining-roles.html#roles-indices-priv).|
    |privileges|read|The read-only permissions on the indexes. The read-only permissions include the permissions to call the count, explain, get, mget, scripts, search, and scroll APIs. For more information, see [privileges-list-indices](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/security-privileges.html#privileges-list-indices).|
    |create\_index|The permission to create indexes. If you specify an alias when you create an index, you must grant the manage permission to the user. **Note:** The alias must meet the matching rules that are defined by the Indices parameter. |
    |view\_index\_metadata|The read-only permissions on index metadata. The permissions include the permissions to call the following APIs: aliases, aliases exists, get index, exists, field mappings, mappings, search shards, type exists, validate, warmers, settings, and ilm.|
    |write|The permission to perform all write operations on documents. The operations include indexing, updates, deletion, bulk operations, and mapping updates. The write permission involves more operation permissions than the create and index permissions.|
    |monitor|The permission to monitor all operations. The operations include the operations that you performed by calling the index recovery, segments info, index stats, or status API.|
    |delete|The permission to delete documents.|
    |delete\_index|The permission to delete indexes.|
    |granted fields|\*|The fields that you want to authorize. The value \* indicates all fields.|
    |**Kibana privileges**|privileges|read|The read-only permissions on Kibana. The permissions are granted to all spaces. Default value: none. This value indicates that no spaces are authorized to access Kibana. **Note:** Versions earlier than Kibana V7.0 support only [base privileges](https://www.elastic.co/guide/en/kibana/6.7/kibana-privileges.html#kibana-privileges). Kibana V7.0 and later support base privileges and [feature privileges](https://www.elastic.co/guide/en/kibana/7.10/kibana-privileges.html#kibana-feature-privileges). After you assign a base privilege to a role, the role has the access permissions on all Kibana spaces. After you assign a feature privilege to a role, the role has the access permissions only on a specific feature. To assign a feature privilege, you must specify a Kibana space. |

-   Verification

    Use the common user to log on to the Kibana console and run the following commands. The system returns results as expected.

    ```
    GET /_cat/indices?v
    ```

    ```
    GET /_cluster/stats
    ```

    ```
    GET /product_info/_search
    ```

    ```
    GET /product_info1/_search
    ```

    ```
    POST /kibana_sample_data_logs/_doc/2
    {
        "productName": "testpro",
        "annual_rate": "3.22%",
        "describe": "testpro"
    }
    ```

    ```
    PUT /product_info2/_doc/1
    {
        "productName": "testpro",
        "annual_rate": "3.22%",
        "describe": "testpro"
    }
    ```

    ```
    DELETE product_info
    ```

    ![Verification](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4511461161/p200462.png)


