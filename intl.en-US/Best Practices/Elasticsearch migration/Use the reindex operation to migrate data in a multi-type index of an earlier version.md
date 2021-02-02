# Use the reindex operation to migrate data in a multi-type index of an earlier version

This topic describes how to use the reindex operation to migrate data from a multi-type index to a single-type index. The multi-type index is on an Alibaba Cloud Elasticsearch V5.X cluster. The single-type index is on an Alibaba Cloud Elasticsearch V6.X cluster.

## Procedure

1.  [Preparations](#section_vdv_hqt_jjm)

    Create an Elasticsearch V5.X cluster, an Elasticsearch V6.X cluster, and a Logstash cluster in the same virtual private cloud \(VPC\).

    -   The Elasticsearch clusters are used to store index data.
    -   The Logstash cluster is used to migrate processed data based on pipelines.
2.  [Step 1: Convert the multi-type index into one or more single-type indexes](#section_y7t_p2z_5is)

    Use the reindex operation to convert the multi-type index on the Elasticsearch V5.X cluster into one or more single-type indexes. You can use one of the following methods to implement the conversion:

    -   Combine types: Call the reindex operation with the script condition specified to combine the types of the index.
    -   Split the index: Call the reindex operation to split the index into multiple indexes. Each of these indexes has only one type.
3.  [Step 2: Use Logstash to migrate data](#section_qih_65i_yb0)

    Use the Logstash cluster to migrate the processed index data to the Elasticsearch V6.X cluster.

4.  [Step 3: View the data migration results](#section_eyh_unl_ywa)

    View the migrated data in the Kibana console.


## Preparations

1.  Create an Elasticsearch V5.5.3 cluster and an Elasticsearch V6.7.0 cluster. Then, create a multi-type index on the Elasticsearch V5.5.3 cluster and insert data into the index.

    For more information about how to create an Elasticsearch cluster, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

2.  Create an Logstash cluster in the VPC where the Elasticsearch clusters reside.

    For more information, see [Create an Alibaba Cloud Logstash cluster](/intl.en-US/Logstash/Quick Start/Step 1: Create a Logstash cluster/Create an Alibaba Cloud Logstash cluster.md).


## Step 1: Convert the multi-type index into one or more single-type indexes

In the following steps, the types of the index are combined to convert the index into one single-type index.

1.  Enable the Auto Indexing feature for the Elasticsearch V5.5.3 cluster.

    1.  Log on to the [Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

    2.  In the left-side navigation pane, click **Elasticsearch Clusters**.

    3.  In the top navigation bar, select a resource group and a region.

    4.  On the **Clusters** page, find the Elasticsearch V5.5.3 cluster and click its ID.

    5.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

    6.  Click **Modify Configuration** on the right side of **YML Configuration**.

    7.  In the **YML File Configuration** panel, set **Auto Indexing** to **Enable**.

        ![Enable the Auto Indexing feature](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9835133951/p74665.png)

        **Warning:** This operation will restart the cluster. Therefore, before you change the value of **Auto Indexing**, make sure that the restart does not affect your services.

    8.  Select **This operation will restart the cluster. Continue?** and click **OK**.

2.  Log on to the Kibana console of the Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

3.  In the left-side navigation pane, click **Dev Tools**.

4.  On the **Console** tab of the page that appears, run the following command to combine the types of the index:

    ```
    POST _reindex
    {
      "source": {
        "index": "twitter"
      },
      "dest": {
        "index": "new1"
      },
      "script": {
        "inline": """
        ctx._id = ctx._type + "-" + ctx._id;
        ctx._source.type = ctx._type;
        ctx._type = "doc";
        """,
        "lang": "painless"
      }
    }
    ```

    In this example, a custom type field is added for the new1 index. ctx.\_source.type specifies the custom type field, and this field is set to the value of the original \_type parameter. In addition, \_id of the new1 index includes \_type-\_id. This prevents documents of different types from having the same ID.

5.  Run the `GET new1/_mapping` command to view the mapping after the combination.

6.  Run the following command to view data in the new index with types combined:

    ```
    GET new1/_search
    {
       "query":{
         "match_all":{
          }
      }
    }
    ```


In the following steps, the multi-type index is split into multiple single-type indexes.

1.  On the **Console** tab, run the following command to split the multi-type index into multiple single-type indexes:

    ```
    POST _reindex
    {
      "source": {
        "index": "twitter",
        "type": "tweet",
        "size": 10000
      },
      "dest": {
        "index": "twitter_tweet"
      }
    }
    POST _reindex
    {
      "source": {
        "index": "twitter",
        "type": "user",
        "size": 10000
      },
      "dest": {
        "index": "twitter_user"
      }
    }
    ```

    In this example, the twitter index is split into the twitter\_tweet and twitter\_user indexes based on types.

2.  Run the following command to view data in the new indexes:

    ```
    GET twitter_tweet/_search
    {
       "query":{
         "match_all":{
    
          }
      }
    }
    ```

    ```
    GET twitter_user/_search
    {
       "query":{
         "match_all":{
    
          }
      }
    }
    ```


## Step 2: Use Logstash to migrate data

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the left-side navigation pane, click **Logstash Clusters**.

3.  In the top navigation bar, select a region. On the **Clusters** page, click the ID of the desired cluster.

4.  In the left-side navigation pane of the page that appears, click **Pipelines**.

5.  In the **Pipelines** section, click **Create Pipeline**.

    For more information about how to create and configure a pipeline, see [Use configuration files to manage pipelines](/intl.en-US/Logstash/Pipeline task management/Use configuration files to manage pipelines.md). The following example configurations are used in this topic:

    ```
    input {
        elasticsearch {
        hosts => ["http://es-cn-0pp1f1y5g000h****.elasticsearch.aliyuncs.com:9200"]
        user => "elastic"
        index => "*"
        password => "your_password"
        docinfo => true
      }
    }
    filter {
    }
    output {
      elasticsearch {
        hosts => ["http://es-cn-mp91cbxsm000c****.elasticsearch.aliyuncs.com:9200"]
        user => "elastic"
        password => "your_password"
        index => "test"
      }
    }
    ```

6.  Click Save and Deploy to start data migration.

    For more information, see [Use configuration files to manage pipelines](/intl.en-US/Logstash/Pipeline task management/Use configuration files to manage pipelines.md).


## Step 3: View the data migration results

1.  Log on to the Kibana console of the Elasticsearch V6.7.0 cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to view the index that stores the migrated data:

    ```
    GET _cat/indices?v
    ```


