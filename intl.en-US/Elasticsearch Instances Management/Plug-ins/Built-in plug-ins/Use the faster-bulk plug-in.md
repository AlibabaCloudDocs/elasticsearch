# Use the faster-bulk plug-in

The faster-bulk plug-in is developed by the Alibaba Cloud Elasticsearch team. This plug-in aggregates bulk write requests in batches based on the specified maximum request size and aggregation interval. This prevents small bulk requests from blocking the write queue, improves write throughput, and reduces write request rejections. This topic introduces the use scenarios of the plug-in and describes how to use it.

## Scenarios

The faster-bulk plug-in is ideal for scenarios with high write throughput and numerous index shards. The plug-in can improve the write throughput by more than 20% in these scenarios.

**Note:** The faster-bulk plug-in aggregates bulk write requests in batches before it writes data to shards. Therefore, we recommend that you do not use this plug-in in latency-sensitive scenarios.

-   Test environment
    -   Node configuration: 3 data nodes and 2 independent client nodes. Each node offers 16 vCPUs and 64 GiB of memory.
    -   Dataset: nyc\_taixs provided by Rally in open source Elasticsearch. The size of a single document is 650 bytes.
    -   Parameter setting: The apack.fasterbulk.combine.interval parameter is set to 200ms.
    -   Translog status: Tests are performed in each of the synchronous and asynchronous states. If the index.translog.durability parameter is set to request, translogs are in the synchronous state. If the index.translog.durability parameter is set to async, translogs are in the asynchronous state.
-   Test results

    |Translog status|Write performance of an open source Elasticsearch cluster without faster-bulk \(document/s\)|Write performance of an Alibaba Cloud Elasticsearch cluster with faster-bulk \(document/s\)|Improved By|
    |---------------|--------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|-----------|
    |Synchronous|182314/s|226242/s|23%|
    |Asynchronous|218732/s|241060/s|10%|

-   Test conclusion

    The write performance is improved in both the synchronous state \(default state\) and asynchronous state after the faster-bulk plug-in is used. In the synchronous state, the write performance is improved by 23%.


## Prerequisites



-   An Alibaba Cloud Elasticsearch V6.7.0 or V7.10.0 cluster is created.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

    **Note:** Only Alibaba Cloud Elasticsearch V6.7.0 and V7.10.0 clusters of the Standard or Advanced Edition support the faster-bulk plug-in.

-   The faster-bulk plug-in is installed.

    For more information, see [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Install and remove a built-in plug-in.md). After the plug-in is installed, the bulk request aggregation feature is disabled by default. Before you use this plug-in, enable this feature.


## Enable the bulk request aggregation feature

1.  Log on to the Kibana console of your Alibaba Cloud Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to enable the bulk request aggregation feature:

    ```
    PUT _cluster/settings
    {
       "transient" : {
          "apack.fasterbulk.combine.enabled":"true"
       }
    }
    ```

    **Note:** You can also use the [cURL tool](/intl.en-US/Developer Guide/Use API operations to manage an Alibaba Cloud Elasticsearch cluster.md) or a third-party visualizer to run the preceding command.


## Configure the maximum request size and aggregation interval

Run the following command to configure the maximum request size and aggregation interval. If the total size of bulk requests or the aggregation interval on a single data node reaches the configured threshold, the system writes data to shards.

```
PUT _cluster/settings
{
   "transient" : {
      "apack.fasterbulk.combine.flush_threshold_size":"1mb",
      "apack.fasterbulk.combine.interval":"50"
   }
}
```

-   apack.fasterbulk.combine.flush\_threshold\_size: the maximum size of bulk requests. Default value: 1mb.
-   apack.fasterbulk.combine.interval: the maximum interval at which bulk requests are aggregated. Default value: 50. Unit: ms.

**Note:** To process highly concurrent bulk requests and prevent the requests from blocking the write queue, you can increase the maximum request size or aggregation interval based on your business requirements.

## Disable the bulk request aggregation feature

Run the following command to disable the bulk request aggregation feature:

```
PUT _cluster/settings
{
   "transient" : {
      "apack.fasterbulk.combine.enabled":"false"
   }
}
```

