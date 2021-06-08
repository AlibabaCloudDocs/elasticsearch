---
keyword: [scenario-based configuration, Dynamic Cluster Configuration, Index Template Configuration, Index Lifecycle Configuration]
---

# Use a scenario-based template to modify the configurations of a cluster

Alibaba Cloud Elasticsearch provides a scenario-based configuration feature. This feature allows you to use the scenario-based templates that are provided by Alibaba Cloud Elasticsearch to modify the configurations of your Alibaba Cloud Elasticsearch cluster. These templates help you achieve optimal cluster and index configurations and avoid cluster exceptions and performance issues that are caused by incorrect configurations. Before you use the templates, you must specify a scenario. General, data analysis, database acceleration, and search scenarios are supported. This topic describes how to use a scenario-based template to modify the configurations of a cluster.

Before you use a scenario-based template to modify the configurations of a cluster, take note of the following items:

-   Different versions and types of clusters support different templates. The templates available in the console take precedence.
-   Standard Edition clusters support general, data analysis, database acceleration, and search scenarios. Advanced Edition clusters support only logging scenarios.
-   The default index template is named aliyun\_default\_index\_template. This template has a low order value and does not affect your custom index templates.
-   The policy defined in the default index lifecycle template is named aliyun\_default\_ilm\_policy. This policy has been applied to the default index template aliyun\_default\_index\_template.
-   When you purchase a cluster, you can select a scenario on the buy page. The default scenario for a Standard Edition cluster is General and that for an Advanced Edition cluster is Logging. After a cluster is purchased, the system automatically applies the configurations in related templates to the cluster.

    **Note:** If **Scenario** is **None**, you can set it based on the following steps. If you set Scenario to a value other than None, you must submit the related templates before they can be applied to the cluster.


1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

3.  Navigate to the desired cluster.

    1.  In the top navigation bar, select a resource group and a region.

    2.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the **Elasticsearch Clusters** page, find the desired cluster and click its ID.

4.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

5.  In the **Scenario-based Configuration** section, click **Modify** on the right side of **Scenario**.

    ![Select a scenario](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0567819951/p104568.png)

6.  In the **Select Scenario** dialog box, select a scenario from the **Scenario** drop-down list and click **OK**.

    **Note:** Modifications in the Scenario-based Configuration section immediately take effect. You do not need to restart your cluster.

7.  Use scenario-based templates to modify the configurations of your cluster.

    ![Modify the configurations based on the scenario you selected](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0567819951/p104560.png)

    -   Dynamic Cluster Configuration: allows you to dynamically modify cluster configurations. Dynamic Cluster Configuration functions the same as the `PUT /_cluster/settings` command. For more information, see [Cluster update settings](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/cluster-update-settings.html).
    -   Index Template Configuration: defines the template that is automatically used only when an index is created. Index Template Configuration functions the same as the `PUT _template/aliyun_default_index_template` command. The modifications of the index template do not affect existing indexes. For more information, see [Index Templates](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/indices-templates.html#indices-templates).
    -   Index Lifecycle Configuration: supports clusters of V6.7.0 or later, regardless of whether the clusters are Standard or Advanced Edition clusters. Index Lifecycle Configuration functions the same as the `PUT _ilm/policy/aliyun_default_ilm_policy` command. For more information, see [Setting up a new policy](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/_setting_up_a_new_policy.html#_setting_up_a_new_policy).
    The following substeps demonstrate how to modify the configurations in an index template.

    1.  Click **Index Template Configuration**.

    2.  In the **Index Template Configuration** panel, click **Apply**.

        ![Apply](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1567819951/p104563.png)

        The following features are provided:

        -   **Apply**: applies the configurations in the Template section to the Current Configuration section. Then, you can modify the configurations.
        -   **Compare**: compares the configurations in the Current Configuration section with those in the Template section. If no modifications are made, this feature is unavailable. During the comparison, you cannot modify the configurations.
        -   **Reset**: restores the configurations in the Current Configuration section to the original values.
    3.  In the **Current Configuration** section, modify the configurations based on your business requirements.

    4.  Click **Compare**.

        ![View comparison results](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1567819951/p104565.png)

    5.  Click **Cancel**.

8.  Click **Submit**.

    After you click Submit, the modifications immediately take effect on your cluster.


