# Use the pruning feature for a time series index

When you query data from a time series index, you can specify a time range to filter the data. The larger the data volume, the longer the query latency. You can use the pruning feature to improve query performance. This topic describes how to use the pruning feature.

An Alibaba Cloud Elasticsearch V6.7.0 or V7.10.0 cluster is created. The kernel version of the V6.7.0 cluster is 1.2.0 or later. For more information about how to create a cluster, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

The pruning feature improves segment merging and query performance. The following table compares open source Elasticsearch with Alibaba Cloud Elasticsearch in terms of the segment merging policy and query performance.

|Service|Segment merging policy|Query performance|
|-------|----------------------|-----------------|
|Open source Elasticsearch|Index segments are merged based on their sizes. Index segments that have similar sizes are merged. This method is efficient but cannot ensure data continuity.|All data is scanned for a query, which causes serious performance loss.|
|Alibaba Cloud Elasticsearch \(with the pruning feature enabled\)|A time series field is added when an index is created. Index segments are merged based on their sizes and the time series field. Index segments that have similar sizes and contain data in adjacent time periods are merged. This improves data continuity.|Data is pruned based on the time range specified in a query, which improves query performance by 40%.|

**Note:** You can run all the commands that are provided in this topic in the Kibana console. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

## Precautions

-   The pruning feature is available only for Alibaba Cloud Elasticsearch V6.7.0 clusters whose kernel versions are V1.2.0 or later and V7.10.0 clusters.
-   You must enable the pruning feature for an index when you create the index. If you enable the feature after an index is created, the expected query performance cannot be achieved.
-   After you disable the pruning feature for an index, we recommended that you do not enable the feature again for the index. If you enable it again and the segment merging of the index covers non-time series data, the expected query performance cannot be achieved.

## Enable the pruning feature for an index

To enable the pruning feature, specify a time series field when you create an index. The following code uses timestamp as the time series field:

```
PUT index-1/_settings
{
    "index" : {
        "merge.policy.time_series_field" : "timestamp"
    }
}
```

**Note:** The data type of the time series field must be DATE or LONG.

## Query data based on the specified time series field

After you run the following command, the system filters data based on the timestamp field and then searches for data.

```
POST index-1/_search
{
    "query": {
        "bool": {
            "filter": [
                {
                    "range": {
                        "timestamp": {
                            "format": "yyyy-MM-dd HH:mm:ss",
                            "gte": "2020-06-01 23:00:00",
                            "lt": "2020-06-06 23:05:00",
                            "time_zone": "+08:00"
                        }
                    }
                },
                {
                    "terms": {
                        "region": [
                            "sh"
                        ]
                    }
                }
            ]
        }
    }
}
```

## Disable the pruning feature for an index

1.  Disable the index.

    ```
    POST index-1/_close
    ```

2.  Update the `settings` of the index to disable the pruning feature.

    ```
    PUT index-1/_settings
    {
        "index" : {
            "merge.policy.time_series_field" : null
        }
    }
    ```

3.  Enable the index again.

    ```
    POST index-1/_open
    ```


