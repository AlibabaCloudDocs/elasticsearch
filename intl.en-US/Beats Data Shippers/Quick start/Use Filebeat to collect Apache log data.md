---
keyword: use Beats to collect log data
---

# Use Filebeat to collect Apache log data

If you want to view and analyze Apache log data, you can use Filebeat to collect the data. Then, use Alibaba Cloud Logstash to filter the data and transfer the processed data to an Alibaba Cloud Elasticsearch cluster for analytics.

-   An Elasticsearch cluster and a Logstash cluster are created in the same Virtual Private Cloud \(VPC\). The versions of the clusters are the same.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md) and [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).

-   The **Auto Indexing** feature is enabled for the Elasticsearch cluster.

    For security purposes, Alibaba Cloud Elasticsearch disables the **Auto Indexing** feature by default. However, Beats depends on this feature. If you select **Elasticsearch** for **Output** when you create a shipper, you must enable the **Auto Indexing** feature. For more information, see [t1605396.md\#section\_pcn\_1xy\_1l2](/intl.en-US/Elasticsearch Instances Management/Access and configure an Elasticsearch cluster.md).

-   An Alibaba Cloud Elastic Compute Service \(ECS\) instance is created in the same VPC as the Elasticsearch cluster and Logstash cluster.

    For more information, see [Create an instance by using the wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the wizard.md).

    **Note:** Beats supports only Aliyun Linux, Red Hat Linux, and CentOS.

-   HTTP Daemon \(HTTPd\) is installed on the ECS instance.

    To facilitate the analytics and display of Apache log data by using a visualization tool, we recommend that you define JSON as the format of the log data in the httpd.conf file. For more information, see [official Apache documentation](https://cwiki.apache.org/confluence/display/httpd/Apache). The following configurations are used in this topic:

    ```
    LogFormat "{\"@timestamp\":\"%{%Y-%m-%dT%H:%M:%S%z}t\",\"client_ip\":\"%{X-Forwa rded-For}i\",\"direct_ip\": \"%a\",\"request_time\":%T,\"status\":%>s,\"url\":\"%U%q\",\"method\":\"%m\",\"http_host\":\"%{Host}i\",\"server_ip\":\"%A\",\"http_referer\":\"%{Referer}i\",\"http_user_agent\":\"%{User-agent}i\",\"body_bytes_sent\":\"%B\",\"total_bytes_sent\":\"%O\"}"  access_log_json
    # Change the original CustomLog configuration to CustomLog "logs/access_log" access_log_json.
    ```

-   Cloud Assistant and Docker are installed on the ECS instance.

    For more information, see [Install the Cloud Assistant client](/intl.en-US/Deployment & Maintenance/Cloud assistant/Configure the Cloud Assistant client/Install the Cloud Assistant client.md) and [Deploy and use Docker on Alibaba Cloud Linux 2 instances](/intl.en-US/Tutorials/Build an application/Deploy and use Docker/Deploy and use Docker on Alibaba Cloud Linux 2 instances.md).


## Procedure

1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the **Create Shipper** section, click **Filebeat**.

3.  Install and configure the shipper.

    For more information, see [Collect the logs of an ECS instance](/intl.en-US/Beats Data Shippers/Collect the logs of an ECS instance.md) and [Prepare the YML configuration files for a shipper](/intl.en-US/Beats Data Shippers/Prepare the YML configuration files for a shipper.md). The following figure shows the configurations that are used in this topic.

    ![Configure Filebeat](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6077853951/p82408.png)

    **Note:**

    -   Set Output to the ID of the Logstash cluster. You do not need to configure Output in the YML file configuration.
    -   Set Filebeat Log File Path to the path that stores the data source. In the YML file configuration, enable log collection and configure the path that is used to store log data.
4.  Click **Next**.

5.  In the **Install Shipper** step, select the target ECS instance.

    ![Select the target ECS instance](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0657359951/p82419.png)

    **Note:** The selected ECS instance must meet the preceding prerequisites.

6.  Enable the shipper and check whether the shipper installation succeeds.

    1.  Click **Enable**.

        Then, the **Enable Shipper** message appears.

    2.  Click **Back to Beats Shippers**. In the **Manage Shippers** section of the **Beats Data Shippers** page, view the installed shipper.

    3.  After the state of the shipper changes to **Enabled 1/1**, click **View Instances** in the **Actions** column.

    4.  In the **View Instances** pane, check whether the shipper installation on the ECS instance succeeds. If the value of **Installed Shippers** is **Heartbeat Normal**, the shipper installation succeeds.

        ![View shipper installation status](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0657359951/p82426.png)

7.  Create a Logstash pipeline.

    1.  In the left-side navigation pane, click **Logstash Clusters**.

    2.  On the page that appears, find the target Logstash cluster and click **Manage Pipelines** in the **Actions** column.

    3.  In the **Pipelines** section of the page that appears, click **Create Pipeline**.

    4.  Configure the pipeline.

        For more information, see [Use configuration files to manage pipelines](/intl.en-US/Logstash/Pipeline task management/Use configuration files to manage pipelines.md). The following configurations are used in this topic:

        ```
        input {
          beats {
              port => 8000
            }
        }
        filter {
          json {
                source => "message"
                remove_field => "@version"
                remove_field => "prospector"
                remove_field => "beat"
                remove_field => "source"
                remove_field => "input"
                remove_field => "offset"
                remove_field => "fields"
                remove_field => "host"
                remove_field => "message"
              }
        
        }
        output {
          elasticsearch {
            hosts => ["http://es-cn-mp91cbxsm00******.elasticsearch.aliyuncs.com:9200"]
            user => "elastic"
            password => "<your_password>"
            index => "<your_index>"
          }
        }
        ```

        |Parameter|Description|
        |---------|-----------|
        |`input`|Receives data collected by the shipper.|
        |`filter`|Filters collected data. The `json` plug-in is used to decode message data. The `remove_field` parameter specifies the field that will be deleted. **Note:** The configurations in the `filter` part apply only to the current testing scenario. You can configure the `filter` part based on your business needs. For information about supported `filter` plug-ins, see [Filter plugins](https://www.elastic.co/guide/en/logstash/6.7/filter-plugins.html#filter-plugins). |
        |`output`|Transfers data to your Elasticsearch cluster. Replace `<your_password>` with the password that is used to access your Elasticsearch cluster. Replace `<your_index>` with the name of the index to which the data is transferred. |


## View the collected data

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to view the collected data:

    ```
    GET <your_index>/_search
    ```

    **Note:** Replace `<your_index>` with the index name that you configured in the output part for the Logstash pipeline.

4.  In the left-side navigation pane, click **Discover**. On the page that appears, specify a period in the upper-right corner. Then, view the details of the collected data within the specified period.

    ![View the details of the collected data](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6077853951/p82431.png)

    **Note:** Before you view the collected data, make sure that an index pattern is created for the index specified by `<your_index>`. To create an index pattern in the Kibana console, click **Management** in the left-side navigation pane. On the page that appears, click **Index Patterns** in the **Kibana** section and then click **Create index pattern**. Follow the instructions to create the index pattern.


