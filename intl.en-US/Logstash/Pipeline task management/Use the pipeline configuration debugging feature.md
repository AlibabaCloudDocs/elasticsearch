---
keyword: [Logstash pipeline configuration debugging, debug log for the Logstash pipeline configuration]
---

# Use the pipeline configuration debugging feature

If the configuration of a Logstash pipeline is incorrect, the output data of the pipeline may not meet requirements. In this case, you must repeatedly check the format of the data on the destination and modify the pipeline configuration in the console. This increases time and labor costs. To address this issue, you can use the pipeline configuration debugging feature provided by Logstash. This feature allows you to view the output data of your Logstash pipeline in the console after you create and deploy the pipeline. This reduces your debugging costs. This topic describes how to use the feature.

The logstash-output-file\_extend plug-in is installed. For more information, see [Install a Logstash plug-in](/intl.en-US/Logstash/Plug-ins/Install a Logstash plug-in.md).

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane of the page that appears, click **Pipelines**.

5.  Click **Create Pipeline**.

6.  Configure and start a pipeline.

    1.  In the **Config Settings** step, specify **Pipeline ID** and **Config Settings**.

        ![Config Settings](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3016219061/p127138.png)

        |Parameter|Description|
        |---------|-----------|
        |**Pipeline ID**|Customize a pipeline ID. The pipeline ID is automatically mapped to the path field of the file\_extend parameter.|
        |**Config Settings**|**Config Settings** consists of the following parts:         -   input: specifies the data source. Open source Logstash [input plug-ins](https://www.elastic.co/guide/en/logstash/6.7/input-plugins.html) are supported, except the file plug-in.
        -   filter: processes the data collected from the data source. Numerous [filter plug-ins](https://www.elastic.co/guide/en/logstash/6.7/filter-plugins.html) are supported.
        -   output: sends the processed data to the destination. In addition to open source [Logstash output plug-ins](https://www.elastic.co/guide/en/logstash/6.7/output-plugins.html), Alibaba Cloud Logstash provides the file\_extend output plug-in. This plug-in also allows you to enable the pipeline configuration debugging feature. After the pipeline is deployed, you can view the output data of the pipeline in the console and check whether the data meets requirements. If the data does not meet requirements, modify the pipeline configuration. |

        Configuration example:

        ```
        input {
             elasticsearch {
               hosts => "http://es-cn-0pp1jxv000****.elasticsearch.aliyuncs.com:9200"
               user  => "elastic"
               index => "twitter"
               password => "<your_password>"
               docinfo => true
               schedule => "* * * * *"
             }
        
        }
        filter {
        
        }
        output {
            elasticsearch {
            hosts => ["http://es-cn-000000000i****.elasticsearch.aliyuncs.com:9200"]
            user => "elastic"
            password => "<your_password>"
            index => "%{[@metadata][_index]}"
            document_id => "%{[@metadata][_id]}"
        
          }
           file_extend {
             path => "/ssd/1/ls-cn-v0h1kzca****/logstash/logs/debug/test"
           }
        
        }
        ```

        **Note:**

        -   By default, the file\_extend parameter in the output part is commented out. If you want to use the pipeline configuration debugging feature, uncomment the parameter.
        -   By default, the path indicated by the path field of the file\_extend parameter is specified by the system. You are not allowed to change the path. You can click **Start Configuration Debug** to obtain the path.
        -   \{pipelineid\} in the path indicated by the path field is automatically mapped to the pipeline ID that you specify. You are not allowed to modify \{pipelineid\}. Otherwise, debug logs cannot be obtained.
    2.  Click **Next**. In the Pipeline Parameters step, configure pipeline parameters.

        For more information about the parameters, see [Use configuration files to manage pipelines](/intl.en-US/Logstash/Pipeline task management/Use configuration files to manage pipelines.md).

    3.  Save the settings and deploy the pipeline.

        -   **Save**: After you click this button, the system stores the pipeline settings and triggers a cluster change. However, the settings do not take effect. After you click Save, the **Pipelines** page appears. In the **Pipelines** section, find the created pipeline and click **Deploy** in the **Actions** column. Then, the system restarts the Logstash cluster to make the settings take effect.
        -   **Save and Deploy**: After you click this button, the system restarts the Logstash cluster to make the settings take effect.
    4.  In the message that appears, click **OK**.

7.  View the debug logs of the pipeline.

    1.  After the cluster is restarted, find the pipeline in the **Pipelines** section and click **View Debug Logs** in the **Actions** column.

    2.  On the **Debug Log** tab of the **Logs** page, view the output data of the pipeline.

        If you have multiple pipelines, you can enter pipelineId: <Pipeline ID\> in the search box to search for the logs of the pipeline.

        ![View the debug logs of the pipeline](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3016219061/p127155.png)


