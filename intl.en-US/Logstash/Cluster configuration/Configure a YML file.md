---
keyword: configure a YML file for a Logstash cluster
---

# Configure a YML file

You can use the YML Configuration function to modify the parameter values in a YML file. These parameters are used to control the tasks that are running in Alibaba Cloud Logstash.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

5.  On the **Cluster Configuration** page, click **Modify Configuration** on the right of **YML Configuration**.

6.  In the **YML File Configuration** pane, modify the parameter values as required.

    ![YML File Configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5942986061/p61153.png)

    For more information about the parameters, see [Logstash 6.7.0 Reference](https://www.elastic.co/guide/en/logstash/6.7/logstash-settings-file.html)

    -   By default, the slow query log is configured in the YML file. We recommend that you do not remove this configuration item.
    -   To ensure a stable service, Logstash does not allow you to modify the following parameter values:

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

    **Warning:** After you modify the configuration in the YML file, the system restarts the Logstash cluster for the modification to take effect. Proceed with caution.

7.  Select the **This operation will restart the cluster. Continue?** check box and click **OK**.

    Then, the system restarts the Logstash cluster. During cluster restart, you can view the task progress by referring to [View progress of running cluster tasks](). After the Logstash cluster is restarted, the YML configuration is updated.


