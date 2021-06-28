---
keyword: [Elasticsearch index compression, codec-compression, brotli, zstd]
---

# Use the codec-compression plug-in of the beta version

codec-compression is an index compression plug-in developed by Alibaba Cloud Elasticsearch. It supports brotli and zstd compression algorithms and provides a high compression ratio for indexes. This plug-in significantly reduces index storage costs.

-   An Alibaba Cloud Elasticsearch V6.7.0 cluster is created. For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

    **Note:** The codec-compression plug-in is available only in Alibaba Cloud Elasticsearch V6.7.0.

-   The codec-compression plug-in is installed. It is automatically installed for new clusters.

    You can check whether the codec-compression plug-in is installed on the **Plug-ins** page. If the plug-in is not installed, you must manually install it. For more information, see [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Install and remove a built-in plug-in.md).


The codec-compression plug-in supports brotli and zstd compression algorithms. It is suitable for scenarios in which large amounts of data need to be written or the storage costs for indexes are high, such as in logging and time series data analysis. The following description provides a performance test performed on the plug-in:

-   Test environment
    -   Cluster configuration: 3 data nodes \(each with 16 vCPUs and 64 GiB of memory\) + 2-TiB standard SSD
    -   Datasets: 74-GiB nyc\_taixs of [Rally](https://github.com/elastic/rally/tree/master/esrally) provided by open source Elasticsearch
    -   Index settings: default settings \(You can call the [force merge](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-forcemerge.html#indices-forcemerge) API to perform operations after data is written.\)
-   Test results

    |Compression algorithm|Index size \(GiB\)|TPS \(document/s\)|
    |---------------------|------------------|------------------|
    |LZ4 \(default compression algorithm of Elasticsearch\)|35.5|202,682|
    |best\_compression \(DEFLATE\)|26.4|181,686|
    |brotli|24.4|182,593|
    |zstd|24.6|181,393|

-   Test conclusion

    When the codec-compression plug-in uses brotli or zstd, it achieves a 45% higher compression ratio but experiences a 10% reduction in write performance compared with when it uses LZ4. However, it achieves an 8% higher compression ratio and maintains the same write performance compared with when it uses best\_compression \(DEFLATE\).


## Procedure

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following commands to specify different compression algorithms for an index:

    -   brotli compression algorithm

        ```
        PUT index-1
        {
            "settings": {
                "index": {
                    "codec": "brotli"
                }
            }
        }
        ```

    -   zstd compression algorithm

        ```
        PUT index-1
        {
            "settings": {
                "index": {
                    "codec": "zstd"
                }
            }
        }
        ```


