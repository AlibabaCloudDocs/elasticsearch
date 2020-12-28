---
keyword: configure a YML file for a Logstash cluster
---

# Configure a YML file

You can use the YML Configuration feature to modify the parameters in the YML file of your Logstash cluster. These parameters are used to control the tasks that are running on the cluster.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

5.  On the **Cluster Configuration** page, click **Modify Configuration** on the right of **YML Configuration**.

6.  In the **YML File Configuration** panel, modify the parameters as required.

    ![YML File Configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5942986061/p61153.png)

    For more information about the parameters, see [Logstash 6.7.0 Reference](https://www.elastic.co/guide/en/logstash/6.7/logstash-settings-file.html).

    When you modify the parameters, take note of the following items:

    -   By default, the slow log feature is enabled in the YML file. The feature helps you locate Logstash issues. Therefore, we recommend that you do not delete the configurations for this feature.
    -   To ensure a stable service, Logstash does not allow you to modify the following parameters:

        ```
        node.name
        path.data
        path.config
        http.host
        http.port
        log.level
        path.logs
        path.plugins
        log.format
        path.settings
        pipeline.workers
        xpack.management.enabled
        xpack.management.pipeline.id
        xpack.management.elasticsearch.username
        xpack.management.elasticsearch.password
        xpack.management.elasticsearch.hosts
        xpack.monitoring.enabled
        xpack.monitoring.elasticsearch.username
        xpack.monitoring.elasticsearch.password
        xpack.monitoring.elasticsearch.hosts
        ```

    **Warning:** After you modify the configurations in the YML file, the system restarts the Logstash cluster for the modifications to take effect. Therefore, before you perform the following operations, make sure that your business is not affected.

7.  Select the **This operation will restart the cluster. Continue?** check box and click **OK**.

    Then, the system restarts the Logstash cluster. During the restart, you can view the restart progress. For more information, see [View progress of running cluster tasks](/intl.en-US/Logstash/Cluster management/View progress of running cluster tasks.md). After the Logstash cluster is restarted, the configurations in the YML file are updated.


