# Use the slow query isolation feature

When you use Elasticsearch for queries, you may encounter the following issue: You send a query request to an Elasticsearch cluster, but the query is defined as a slow query. As a result, all the resources on the nodes in the cluster are used for the query, which affects your online business. To address this issue, the Alibaba Cloud Elasticsearch team develops the slow query isolation feature. This feature can be used to track the overheads for a single query request and implement logical separation. If the overheads for the request exceed a specific threshold, the system considers the query as an anomalous query and suspends it. This prevents exceptions caused by a single anomalous query in the cluster and improves cluster stability. This topic describes how to use the slow query isolation feature.

To use the slow query isolation feature, you must configure a resource isolation pool that has a fixed memory size. If the size of the memory used for a single query request exceeds a specific threshold, the query request is directed to the isolation pool for management. If the total size of the memory used by the query requests in the pool exceeds a specific threshold, the system suspends the query requests that consume the most memory based on a priority policy. The priority policy can be adopted by users based on their business requirements.

## Precautions

-   The slow query isolation feature is available for Alibaba Cloud Elasticsearch V6.7.0 clusters whose kernel versions are V1.3.0 and Alibaba Cloud Elasticsearch V7.10.0 clusters.

    **Note:** If the version of your Elasticsearch cluster is V6.7.0, you must make sure that the kernel version of the cluster is V1.3.0 before you use the slow query isolation feature. If the kernel version of the cluster is not V1.3.0, upgrade the kernel. You can upgrade the kernels only of Standard Edition clusters whose kernel versions are V0.3.0, V1.0.2, or V1.2.0. If the version of your Elasticsearch cluster is V7.10.0, you can directly use this feature.

-   The slow query isolation feature is disabled by default. You must enable the feature before you use it.
-   All the commands provided in this topic can be run in the Kibana console. For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

## Procedure

1.  Enable the slow query isolation feature.

    ```
    PUT _cluster/settings
    {
      "persistent": {   
         "search.isolator.enabled": true
       }
    }
    ```

    **Note:** If you want to disable the feature, set search.isolator.enabled to null or false.

2.  Configure thresholds to intercept query requests. If the size or latency of a query request exceeds the related threshold, the query request is directed to the slow query isolation pool.

    ```
    PUT _cluster/settings
    {
       "persistent": {
          "search.isolator.trigger.task.mem_cost": "500mb",  
          "search.isolator.trigger.task.latency": "10s" 
       }
    }
    ```

    |Parameter|Default value|Description|
    |---------|-------------|-----------|
    |search.isolator.trigger.task.mem\_cost|500mb|The threshold for the size of the memory that can be used for a single query request. If the size of the memory that is used for a query request exceeds the threshold, the system directs the query request to the slow query isolation pool.|
    |search.isolator.trigger.task.latency|10s|The threshold for the latency of a query request. If the time spent on a query request exceeds the threshold, the system directs the query request to the slow query isolation pool.|

3.  Configure the thresholds for the total size of the memory that can be used for the query requests in the slow query isolation pool and the maximum number of query requests that can be processed at the same time in the slow query isolation pool. If the total size of the memory used by the query requests in the slow query isolation pool or the number of query requests that are processed at the same time in the slow query isolation pool exceeds the related threshold, the system suspends the query requests that consume the most memory in the isolation pool.

    ```
    PUT _cluster/settings
    {
       "persistent": {
          "search.isolator.total.mem.limit": "60%",
          "search.isolator.total.heap.usage.limit": "75%",
          "search.isolator.total.tasks.limit": 1000 
       }
    }
    ```

    |Parameter|Default value|Description|
    |---------|-------------|-----------|
    |search.isolator.total.mem.limit|60%|The threshold for the proportion of the heap memory that is consumed by the query requests in the slow query isolation pool to the memory of the whole cluster. The default value is 60%. This value indicates that the query requests in the slow query isolation pool are suspended if the proportion reaches 60%.|
    |search.isolator.total.heap.usage.limit|75%|The threshold for the heap memory usage of the cluster. The default value is 75%. This value indicates that the query requests in the slow query isolation pool are suspended if the usage reaches 75%.|
    |search.isolator.total.tasks.limit|1000|The maximum number of query requests that can be processed at the same time in the slow query isolation pool. The default value is 1000. This value indicates that the query requests in the slow query isolation pool are suspended if the number of query requests that are processed at the same time exceeds 1,000.|

4.  View the query requests in the slow query isolation pool.

    ```
    GET _tasks/isolator?detailed=true
    ```

5.  Cancel a query request.

    ```
    POST _tasks/<taskId>/_cancel
    ```

    **Note:** taskId: the ID of the query request.


