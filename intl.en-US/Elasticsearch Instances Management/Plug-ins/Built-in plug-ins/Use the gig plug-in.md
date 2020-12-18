# Use the gig plug-in

gig is a plug-in developed by Alibaba Cloud Elasticsearch to implement throttling for client nodes in an Elasticsearch cluster. This plug-in integrates the core throttling capabilities possessed by the Taobao team to handle searches. The gig plug-in can perform a switchover within seconds if query jitters caused by accidental node exceptions occur. This minimizes the probability that query jitters occur and ensures the stability of queries. In addition, this plug-in detects traffic to handle query latency surges caused by enabled warm nodes and achieve query warm-up for online business. This topic describes how to use the gig plug-in.

## Background information

This section describes how the gig plug-in works.

-   The gig plug-in runs on client nodes. For applications that require high query QPS, you can increase the number of replica shards for each primary shard to scale out the cluster. This helps achieve a linear increase in query throughput. The gig plug-in can help client nodes select the most appropriate replica shards to provide query services.
-   The plug-in determines the service capabilities of nodes based on query latency and coordinates the nodes that provide services by using the proportion integral differential \(PID\) algorithm. This ensures rapid and accurate coordination. If exceptions such as surging query latency or rising error rates occur on nodes, the gig plug-in can collect and analyze the metrics of the nodes in real time by using the PID algorithm. Then, the plug-in rapidly isolates anomalous nodes and performs a switchover within seconds.
-   When new nodes join the cluster, the plug-in samples online query traffic in real time, replicates some query traffic to the new nodes, and discards query results. The traffic that is replicated is detection traffic. This avoids direct transmission of traffic to nodes that cannot provide services and reduces query latency. If the detection results and metrics show that the latency of the new nodes is in a normal range, the plug-in transmits online query traffic to these nodes. Then, these nodes can provide online services.

## Precautions

-   The gig plug-in is available for Alibaba Cloud Elasticsearch V6.7.0 clusters that have a kernel version of 1.3.0. Before you use this plug-in, make sure that the kernel version of your Elasticsearch cluster is 1.3.0. Otherwise, upgrade the kernel. You can upgrade only the kernels of Standard Edition clusters whose kernel versions are V0.3.0, V1.0.2, or V1.2.0.
-   The gig plug-in is integrated into kernel V1.3.0. After you upgrade the kernel of your cluster, the plug-in is automatically installed. After the plug-in is installed, the throttling feature of the plug-in is disabled by default. If you want to use the feature, you must enable it.
-   Before you use the gig plug-in, make sure that sufficient resources are reserved for the data nodes in the cluster. If exceptions occur on one of the data nodes, the query traffic is transmitted to other data nodes. This increases the load of these nodes. Therefore, you must reserve sufficient resources for data nodes to ensure business stability.
-   All commands in this topic can be run in the Kibana console. For more information about how to log on to the Kibana console, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

## Procedure

1.  Enable the throttling feature for the gig plug-in.

    ```
    PUT test/_settings
    {
     "index.flow_control.enabled": true
    }
    ```

    **Note:** If you want to disable the feature, set index.flow\_control.enabled to null or false.

2.  Configure thresholds for query latency in the gig plug-in. If one of the thresholds are met, the plug-in performs throttling.

    ```
    PUT test/_settings
    {
        "index.flow_control.search": {
                "latency_upper_limit_extra": "10s", 
                "latency_upper_limit_extra_percent": "1.0", 
                "probe_percent": "0.2",
                "full_degrade_error_percent": "0.5", 
                "full_degrade_latency": "10s" 
        }
    }
    ```

    |Parameter|Default value|Description|
    |---------|-------------|-----------|
    |latency\_upper\_limit\_extra|10s|The threshold for the absolute value of the difference between the actual query latency and average query latency. This parameter is represented by using the following formula: \|Actual query latency - Average query latency\|. The default value is 10s. This indicates that if the average query latency of three data nodes in the cluster is 2s, when the query latency of one of the three data nodes reaches 13s, the gig plug-in performs throttling.|
    |latency\_upper\_limit\_extra\_percent|1.0|The threshold for the proportion of the absolute value of the difference between the actual query latency and average query latency to the average query latency. This parameter is represented by using the following formula: \(\|Actual query latency - Average query latency\|\)/Average query latency. The default value is 1.0. This indicates that if the average query latency of three data nodes in the cluster is 2s, when the actual query latency of one of the three data nodes reaches 4s, the gig plug-in performs throttling.|
    |probe\_percent|0.2|The threshold for the proportion of [detection traffic](#section_ad3_tgr_pk8) to the actual query traffic. The default value is 0.2. This indicates that if the proportion of detection traffic to the actual query traffic is greater than 0.2, the gig plug-in performs throttling.|
    |full\_degrade\_error\_percent|0.5|The threshold for the proportion of query exceptions. The default value is 0.5. This indicates that if the error rate of query responses of a data node in the cluster reaches 50%, the gig plug-in performs throttling.|
    |full\_degrade\_latency|10s|The threshold for the query latency. The default value is 10s. This indicates that if the query latency is greater than 10s, the gig plug-in performs throttling.|

    **Note:** You can adjust the values of these parameters based on your business requirements.


