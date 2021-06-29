# FAQ about open source Elasticsearch

This topic provides answers to some frequently asked questions about open source Elasticsearch.

-   [How do I configure the thread pool size for indexes?](#section_prp_6bi_our)
-   [What do I do if OOM occurs?](#section_1de_dy5_3j7)
-   [How do I manually manage a shard?](#section_yfz_s3l_4qh)
-   [What are the cache clearing policies for Elasticsearch?](#section_88t_fp8_8pd)
-   [How do I reroute index shards?](#section_2w1_cfv_mfd)
-   [When I query an index, the statusCode: 500 error message is displayed. What do I do?](#section_hbj_bpj_x88)
-   [How do I change the value of the auto\_create\_index parameter?](#section_l2r_3gz_gjs)
-   [How long is required to create a snapshot that will be stored in OSS?](#section_bqp_vk8_i7d)
-   [How do I specify the number of shards when I create an index?](#section_a71_gt4_1lr)
-   [When I use the elasticsearch-repository-oss plug-in to migrate data from a self-managed Elasticsearch cluster, the following error message is displayed. What do I do?](#section_shq_6ns_3tl)
-   [How do I adjust the Elasticsearch server time?](#section_dg3_ci7_hms)
-   [What type of data can I perform Elasticsearch term queries on?](#section_361_lot_89b)
-   [What are the precautions for using aliases in Elasticsearch?](#section_vpf_hzi_6r6)
-   [How do I delete multiple indexes at a time?](#section_cmt_se0_1v6)

## How do I configure the thread pool size for indexes?

In the YML configuration file of your Elasticsearch cluster, specify the thread\_pool.write.queue\_size parameter to configure the thread pool size. For more information, see [Configure the YML file](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md).

![Configure the thread pool size](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3358158061/p180387.png)

**Note:** For Elasticsearch clusters earlier than 6.X, use the thread\_pool.index.queue\_size parameter to configure the thread pool size.

## What do I do if OOM occurs?

Run the following command to clear the cache. Then, analyze the cause, and upgrade the configuration of your Elasticsearch cluster or adjust your business. For more information about how to upgrade the cluster configuration, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).

```
curl -u elastic:<password> -XPOST "localhost:9200/<index_name>/_cache/clear?pretty"
```

|Parameter|Description|
|---------|-----------|
|`<password>`|The password that is used to access your Alibaba Cloud Elasticsearch cluster. The password is specified when you create the cluster or initialize Kibana.|
|`<index_name>`|The name of the index.|

## How do I manually manage a shard?

Use the reroute API or Cerebro. For more information, see [Cluster reroute API](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/cluster-reroute.html) and [Cerebro](/intl.en-US/Best Practices/Elasticsearch applications/Cluster management/Use Cerebro to access an Elasticsearch cluster.md).

## What are the cache clearing policies for Elasticsearch?

Elasticsearch supports the following cache clearing policies:

-   Clear the cache of all indexes

    ```
    curl localhost:9200/_cache/clear?pretty
    ```

-   Clear the cache of a specific index

    ```
    curl localhost:9200/<index_name>/_cache/clear?pretty
    ```

-   Clear the cache of multiple indexes

    ```
    curl localhost:9200/<index_name1>,<index_name2>,<index_name3>/_cache/clear?pretty
    ```


## How do I reroute index shards?

If some shards are lost or inappropriately allocated, you can run the following command to reroute the shards:

```
curl -XPOST 'localhost:9200/_cluster/reroute' -d '{
    "commands" : [ {
        "move" :
            {
              "index" : "test", "shard" : 0,
              "from_node" : "node1", "to_node" : "node2"
            }
        },
        {
          "allocate" : {
              "index" : "test", "shard" : 1, "node" : "node3"
          }
        }
    ]
}'
```

## When I query an index, the `statusCode: 500` error message is displayed. What do I do?

We recommend that you use a third-party plug-in, such as [Cerebro](/intl.en-US/Best Practices/Elasticsearch applications/Cluster management/Use Cerebro to access an Elasticsearch cluster.md), to query the index.

-   If the query succeeds, the issue is caused by an invalid index name. In this case, change the index name. An index name can contain only letters, underscores \(\_\), and digits.
-   If the query fails, the issue is caused by an error in the index or your cluster. In this case, check whether your cluster stores the index and runs in a normal state.

## How do I change the value of the `auto_create_index` parameter?

Run the following command:

```
PUT /_cluster/settings
{
    "persistent" : {
        "action": {
          "auto_create_index": "false"
        }
    }
}
```

**Note:** The default value of the `auto_create_index` parameter is false. This value indicates that the system does not automatically create indexes. In most cases, we recommend that you do not change the value of this parameter. Otherwise, excessive indexes are created, and the mappings or settings of the indexes do not meet your expectations.

## How long is required to create a snapshot that will be stored in OSS?

If the number of shards, memory usage, disk usage, and CPU utilization of your cluster are normal, about 30 minutes are required to create a snapshot for 80 GB of index data.

## How do I specify the number of shards when I create an index?

You can divide the total data size by the data size of each shard to obtain the number of shards. We recommend that you limit the data size of each shard to 30 GB. If the data size of each shard exceeds 50 GB, query performance is severely affected.

You can appropriately increase the number of shards to speed up index creation. The query performance is affected no matter whether the number of shards is small or large.

-   Shards are stored on different nodes. If the number of shards is set to a large value, more files need to be opened, and more interactions are required among the nodes. This decreases query performance.
-   If the number of shards is set to a small value, each shard stores more data. This also decreases query performance.

## When I use the elasticsearch-repository-oss plug-in to migrate data from a self-managed Elasticsearch cluster, the following error message is displayed. What do I do?

Error message: `ERROR: This plugin was built with an older plugin structure. Contact the plugin author to remove the intermediate "elasticsearch" directory within the plugin zip`

Change the name of the ZIP plug-in package from elasticsearch to elasticsearch-repository-oss, and copy the package to the plugins directory.

## How do I adjust the Elasticsearch server time?

You can change the time zone in the Kibana console. In this example, an Elasticsearch 6.7.0 cluster is used.

![Adjust server time 1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3358158061/p180469.png)

The following figure shows the selected time zone.

![Select a time zone](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3358158061/p180471.png)

## What type of data can I perform Elasticsearch term queries on?

Term queries are word-level queries that can be performed on structured data, such as numbers, dates, and keywords other than text.

**Note:** When you perform a full-text query, the system splits words in the text. When you perform a word-level query, the system directly searches the inverted indexes that contain the related fields. Word-level queries are generally performed on fields of a numeric or date data type.

## What are the precautions for using aliases in Elasticsearch?

The total number of shards for indexes that have the same alias must be less than 1,024.

## How do I delete multiple indexes at a time?

By default, Elasticsearch does not allow you to delete multiple indexes at a time. You can run the following command to enable the deletion. Then, use a wildcard to delete multiple indexes at a time.

```
PUT /_cluster/settings
{
  "persistent": {
     "action.destructive_requires_name": false
  }
}
```

