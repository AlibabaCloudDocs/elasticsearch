# Step 3: View data synchronization results

This topic describes how to view data synchronization results. You can log on to the Kibana console of your destination Alibaba Cloud Elasticsearch cluster to view the results.

Your destination Elasticsearch cluster is configured in the output part of the configuration file for your Logstash pipeline. For more information, see [Step 2: Create and run a data synchronization task](/intl.en-US/Logstash/Quick Start/Step 2: Create and run a data synchronization task.md).

1.  Log on to the Kibana console of your destination Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to view data synchronization results:

    ```
    GET /my_index/_search
    {
      "query": {
        "match_all": {}
      }
    }
    ```

    If the command is executed successfully, the result shown in the following figure is returned.

    ![Command output](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1246186061/p85392.png)


