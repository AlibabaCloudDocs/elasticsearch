# Modify the index lifecycle configurations of a cluster

Alibaba Cloud Elasticsearch provides a default index lifecycle template in the Scenario-based Configuration section of the Cluster Configuration page in the Elasticsearch console. This template enables you to modify the index lifecycle configurations of an Elasticsearch cluster that uses the hot-warm architecture. This template is available in the Elasticsearch console only if your Elasticsearch cluster is of V6.7.0 or later and contains warm nodes. This topic describes the parameters in the template.

For more information about how to modify the index lifecycle configurations of a cluster, see [Use a scenario-based template to modify the configurations of a cluster](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Perform scenario-based configuration/Use a scenario-based template to modify the configurations of a cluster.md). The following table describes the parameters in the template.

**Note:**

-   The policy defined in the default index lifecycle template is named aliyun\_default\_ilm\_policy. By default, this policy is applied to the default index template aliyun\_default\_index\_template. You can use Elasticsearch APIs to query the templates and policy. For more information, see [Getting templates](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/indices-templates.html#getting) and [Get lifecycle policy API](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/ilm-get-lifecycle.html).
-   You can configure other index lifecycle settings for your cluster even if the default index lifecycle template is applied.
-   The default index lifecycle template is unavailable if your cluster is of a version earlier than V6.7.0 or is of V6.7.0 or later but does not contain warm nodes. If you want to manage the index lifecycle of a cluster whose version is earlier than V6.7.0 or a cluster that is of V6.7.0 or later but does not contain warm nodes, you can use the index lifecycle management \(ILM\) feature. For more information about this feature, see [Elasticsearch ILM](https://www.elastic.co/guide/en/elasticsearch/reference/7.13/index-lifecycle-management.html).

|Parameter|Description|
|---------|-----------|
|phases.hot.min\_age|The time that is required for an index to enter the hot phase.|
|phases.hot.actions.set\_priority.priority|The priority of an index in the hot phase.|
|phases.warm.min\_age|The time that is required for an index to enter the warm phase.|
|phases.warm.actions.allocate.number\_of\_replicas|The number of replica shards for an index in the warm phase.|
|phases.warm.actions.allocate.require.box\_type|The shard allocation policy in the warm phase. For example, the system allocates shards to warm nodes.|
|phases.warm.actions.set\_priority.priority|The priority of an index in the warm phase.|
|phases.cold.min\_age|The time that is required for an index to enter the cold phase.|
|phases.cold.actions.set\_priority.priority|The priority of an index in the cold phase.|

