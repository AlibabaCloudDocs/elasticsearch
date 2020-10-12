---
keyword: use Beats to collect metrics
---

# Use user-created Metricbeat to collect system metrics

If you want to view and analyze the metrics of a machine, you can use Metricbeat to collect the metrics. Then, Metricbeat sends the collected data to an Alibaba Cloud Elasticsearch cluster. You can view the data on the related dashboard in the Kibana console of the cluster. This topic uses a computer that runs the macOS operating system to describe the detailed procedure.

You have completed the following operations:

-   An Alibaba Cloud Elasticsearch cluster is created. For more information, see [Create an Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Elasticsearch cluster.md).

    **Note:** If you want to access the Elasticsearch cluster by using its internal endpoint, you must purchase an Elastic Compute Service \(ECS\) instance. This instance must reside in the same virtual private cloud \(VPC\), region, and zone as the Elasticsearch cluster.

-   Metricbeat is downloaded.
    -   [Metricbeat installation package for macOS](https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.0-darwin-x86_64.tar.gz)
    -   [Metricbeat installation package for Linux \(x86\)](https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.0-linux-x86.tar.gz)
    -   [Metricbeat installation package for Linux \(x64\)](https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.0-linux-x86_64.tar.gz)
    -   [Metricbeat installation package for Windows \(x86\)](https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.0-windows-x86.zip)
    -   [Metric installation package for Windows \(x64\)](https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.7.0-windows-x86_64.zip)

Beats is a platform for single-purpose data shippers. After the data shippers are installed, they send data from thousands of machines and systems to Logstash or Elasticsearch.

Metricbeat is a lightweight data shipper that collects metrics from your systems and services. System metrics include CPU and memory metrics. Service metrics include Redis and NGINX metrics.

## Procedure

1.  [Configure an Alibaba Cloud Elasticsearch cluster](#section_llm_6tp_v57)

2.  [Configure Metricbeat](#section_weo_gpf_ixo)

3.  [View the related dashboard in the Kibana console](#section_863_9xl_fyc)

    **Note:** You can also use Metricbeat to collect metrics from a computer that runs the Linux or Windows operating system. Then, Metricbeat sends the metrics to Alibaba Cloud Elasticsearch.


## Configure an Alibaba Cloud Elasticsearch cluster

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click Elasticsearch Clusters. On the **Clusters** page, find your Elasticsearch cluster. Then, click its ID in the **Cluster ID/Name** column or **Manage** in the **Actions** column.

4.  In the left-side navigation pane, click **Security**.

5.  On the page that appears, turn on **Public Network Access** and click **Update** next to **Public Network Whitelist**. Then, enter the public IP address of your computer.

    ![Configure a whitelist for access to the Elasticsearch cluster over the Internet](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1123742061/p40014.png)

    **Note:** If you are using a public network such as Wi-Fi, add the IP address of the jump server that controls outbound traffic of the public network to the whitelist. If you cannot obtain this IP address, we recommend that you add `0.0.0.0/1,128.0.0.0/1` to the whitelist. These two CIDR blocks are used in this topic. This configuration allows almost all public IP addresses to access the Elasticsearch cluster. We recommend that you evaluate the risks before you use this configuration.

6.  In the left-side navigation pane, click **Basic Information**. On the page that appears, you can obtain the public endpoint of your Elasticsearch cluster for future use.

    ![Obtain the public endpoint of your Elasticsearch cluster](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1657359951/p40016.png)

7.  In the left-side navigation pane, click **Cluster Configuration**. On the page that appears, click **Modify Configuration** on the right side of **YML Configuration**. In the YML File Configuration pane, set **Auto Indexing** to **Enable**.

    ![Enable the Auto Indexing feature](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1657359951/p40017.png)

    **Warning:** This configuration takes effect only after your Elasticsearch cluster is restarted. To prevent impacts on your business, exercise caution when you change the settings of Auto Indexing.

8.  Select **This operation will restart the cluster. Continue?** and click **OK**.

    You can view the restart progress in the [Tasks](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the progress of a cluster task.md) dialog box. The configuration of the cluster takes effect after your Elasticsearch cluster is restarted.


## Configure Metricbeat

1.  Decompress the Metricbeat installation package you have downloaded and go to the Metricbeat folder.

    ![Go to the Metricbeat folder](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1657359951/p40018.png)

2.  Open the metricbeat.yml file and edit the `Elasticsearch output` section in it. You must uncomment the involved content.

    ![Edit the metricbeat.yml file](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1657359951/p40019.png)

    |Parameter|Description|
    |---------|-----------|
    |`hosts`|The internal or public endpoint that is used to access the Elasticsearch cluster. The public endpoint is used to access the Elasticsearch cluster in this topic.|
    |`protocol`|Set this parameter to `http`.|
    |`username`|The default value of this parameter is `elastic`.|
    |`password`|The password that is used to access the Elasticsearch cluster. This password is specified when you create the cluster.|

3.  Run the following command to start Metricbeat:

    ```
    ./metricbeat -e -c metricbeat.yml
    ```

    ![Metricbeat started](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1657359951/p40020.png)

    After Metricbeat is started, it begins to send data to your Elasticsearch cluster.


## View the related dashboard in the Kibana console

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Management** and follow these steps to create an index pattern:

    **Note:** If you have created an index pattern, skip this step.

    1.  In the **Kibana** section of the **Management** page, click **Index Patterns**.

    2.  In the **Create index pattern** section, enter an index pattern name \(the name of the index that you want to query\).

    3.  Click **Next step**.

        ![Create an index pattern](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1867819951/p94699.png)

    4.  Click **Create index pattern**.

3.  In the left-side navigation pane, click **Dashboard**.

4.  On the **Dashboards** page, you can view the collected data.

    -   The following figure shows relevant metrics.

        ![List of metrics](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2123742061/p40021.png)

    -   Click **Metricbeat-cpu** to view CPU metrics.

        ![CPU metrics](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2123742061/p40022.png)

        **Note:** You can configure metrics to be refreshed at 5-second intervals. The system generates reports for the metrics collected at these intervals. You can also connect to WebHook to configure alerts.


## References

[Build a visualized operations and maintenance \(O&M\) system with Beats](https://yq.aliyun.com/articles/618611)

