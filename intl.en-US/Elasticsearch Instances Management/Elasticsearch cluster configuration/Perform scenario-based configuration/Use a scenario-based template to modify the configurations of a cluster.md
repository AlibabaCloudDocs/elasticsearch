---
keyword: [scenario-based configuration, dynamic cluster configuration, index template configuration, index lifecycle configuration]
---

# Use a scenario-based template to modify the configurations of a cluster

Alibaba Cloud Elasticsearch provides a scenario-based configuration feature. This feature allows you to use scenario-based templates that are provided by the system to modify the configurations of your Alibaba Cloud Elasticsearch cluster. These templates help you achieve optimal cluster and index configurations and avoid cluster exceptions and performance issues that are caused by incorrect configurations. Before you use these templates, you must specify a scenario. General, data analysis, database acceleration, search, and logging scenarios are supported. This topic describes how to use a scenario-based template to modify the configurations of a cluster.

Before you use a scenario-based template to modify the configurations of a cluster, take note of the following items:

-   Different versions and types of clusters support different templates. The templates available in the console take precedence.
-   Elasticsearch clusters of the Standard Edition support general, data analysis, database acceleration, and search scenarios. Whereas, Elasticsearch clusters of the Advanced Edition support only logging scenarios.
-   The default index template is named aliyun\_default\_index\_template. This template has a low order value and does not affect your custom index templates.
-   The policy defined in the default index lifecycle template is named aliyun\_default\_ilm\_policy. This policy is already applied to aliyun\_default\_index\_template.
-   When you purchase a cluster, you can select a scenario on the buy page. The default scenario for an Elasticsearch cluster of the Standard Edition is General and that for an Elasticsearch cluster of the Advanced Edition is Logging. After a cluster is purchased, the system automatically applies the configurations in related templates to the cluster.

    **Note:** If your cluster is purchased before the scenario-based configuration feature is launched, the feature is disabled for the cluster. You can manually enable the feature as needed. After the feature is enabled, you must manually apply the configurations in the templates to your cluster.


1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  In the top navigation bar, select a resource group and a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

5.  In the **Scenario-based Configuration** section, click **Modify** next to **Scenario**.

    ![Select a scenario](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0567819951/p104568.png)

6.  In the **Select Scenario** dialog box, select a scenario from the **Scenario** drop-down list and click **OK**.

    **Note:** Modifications in the Scenario-based Configuration section take effect immediately without the need to restart your cluster.

7.  Use scenario-based templates to modify the configurations of your cluster.

    ![Modify configurations based on the scenario you selected](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0567819951/p104560.png)

    -   Dynamic Cluster Configuration: allows you to modify the dynamic settings of your cluster. Dynamic Cluster Configuration functions the same as the `PUT /_cluster/settings` command. For more information, see [Cluster Update Settings](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/cluster-update-settings.html).
    -   Index Template Configuration: allows you to modify the index template that is automatically used when an index is created. Index Template Configuration functions the same as the `PUT _template/aliyun_default_index_template` command. Modifying the index template has no impact on existing indexes. For more information, see [Index Templates](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/indices-templates.html#indices-templates).
    -   Index Lifecycle Configuration: allows you to modify the index lifecycle configurations of an Elasticsearch cluster of V6.7 or later that has warm nodes. Index Lifecycle Configuration functions the same as the `PUT _ilm/policy/aliyun_default_ilm_policy` command. For more information, see [Setting up a new policy](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/_setting_up_a_new_policy.html#_setting_up_a_new_policy).
    The following substeps demonstrate how to modify the configurations in an index template.

    1.  Click **Index Template Configuration**.

    2.  In the **Index Template Configuration** pane, click **Apply**.

        ![Apply](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1567819951/p104563.png)

        The following features are provided:

        -   **Apply**: applies the configurations in the Template section to the Current Configuration section. Then, you can modify the configurations.
        -   **Compare**: compares the configurations in the Current Configuration and Template sections. If no modifications are made, this feature is unavailable. During the comparison, you cannot modify configurations.
        -   **Reset**: restores configurations in the Current Configuration section to the original configurations.
    3.  In the **Current Configuration** section, modify modifications as needed.

    4.  Click **Compare**.

        ![View comparison results](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1567819951/p104565.png)

    5.  Click **Cancel**.

8.  Click **Submit**.

    Then, the modifications take effect immediately for your cluster.


