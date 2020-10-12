# Separate hot and cold data

Alibaba Cloud Elasticsearch allows you to separate hot and cold data to improve the efficiency of data queries and reduce the storage costs of historical data. This topic describes how to create an Elasticsearch cluster that uses the hot-warm architecture and specify the hot or warm attribute for indexes to separate hot and cold data.

The cluster that uses the hot-warm architecture contains hot nodes and warm nodes. This architecture improves the performance and stability of your Elasticsearch cluster. The following table lists the differences between hot nodes and warm nodes.

|Node type|Type of data stored|Read and write performance|Specifications|Disk|
|---------|-------------------|--------------------------|--------------|----|
|Hot node|Recent data, such as log data over the last two days.|High|High, such as 32 vCPUs and 64 GiB of memory|We recommend that you use a standard SSD. You can specify the storage space based on the volume of data.|
|Warm node|Historical data, such as log data before the last two days.|Low|Low, such as 8 vCPUs and 32 GiB of memory|We recommend that you use an ultra disk. You can specify the storage space based on the volume of data.|

**Note:** You can use the index lifecycle management \(ILM\) feature to periodically migrate data from hot nodes to warm nodes. For more information, see [ILM overview](https://www.elastic.co/guide/en/elasticsearch/reference/current/overview-index-lifecycle-management.html).

## Create an Alibaba Cloud Elasticsearch cluster that uses the hot-warm architecture

When you [purchase an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create clusters.md), you can purchase warm nodes to create an Elasticsearch cluster that uses the hot-warm architecture. After you create a cluster that contains warm data nodes, the system adds the -Enode.attr.box\_type parameter to the startup parameters of nodes.

-   Hot node: -Enode.attr.box\_type=hot

    **Note:** Data nodes become hot nodes only after you purchase warm nodes.

-   Warm node: -Enode.attr.box\_type=warm

## Specify the hot or warm attribute for indexes

Run the following commands to specify the hot or warm attribute for indexes. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

-   Specify the hot attribute for an index

    ```
    PUT hot_data/_settings
    {
    "index.routing.allocation.require.box_type": "hot"
    }
    ```

-   Specify the warm attribute for an index

    ```
    PUT warm_data/_settings
    {
    "index.routing.allocation.require.box_type": "warm"
    }
    ```


