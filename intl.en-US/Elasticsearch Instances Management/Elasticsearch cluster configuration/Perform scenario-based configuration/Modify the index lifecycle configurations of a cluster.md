# Modify the index lifecycle configurations of a cluster

If you purchase warm nodes and enable the scenario-based configuration feature for your cluster, you can dynamically modify the index lifecycle configurations of the cluster. You can modify index lifecycle configurations only for clusters of V6.7.0 or later, regardless of whether the clusters are Standard or Advanced Edition clusters. This topic describes the parameters related to the index lifecycle configurations of a cluster.

For more information about how to modify the index lifecycle configurations of a cluster, see [Use a scenario-based template to modify the configurations of a cluster](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Perform scenario-based configuration/Use a scenario-based template to modify the configurations of a cluster.md). The following table describes the related parameters.

**Note:** The policy defined in the default index lifecycle template is named aliyun\_default\_ilm\_policy. This policy has been applied to the default index template aliyun\_default\_index\_template.

|Parameter|Description|
|---------|-----------|
|phases.hot.min\_age|The time required for an index to enter the hot phase.|
|phases.hot.actions.set\_priority.priority|The priority of an index in the hot phase.|
|phases.warm.min\_age|The time required for an index to enter the warm phase.|
|phases.warm.actions.allocate.number\_of\_replicas|The number of replica shards for an index in the warm phase.|
|phases.warm.actions.allocate.require.box\_type|The shard allocation policy in the warm phase. For example, the system allocates shards to warm nodes.|
|phases.warm.actions.set\_priority.priority|The priority of an index in the warm phase.|
|phases.cold.min\_age|The time required for an index to enter the cold phase.|
|phases.cold.actions.set\_priority.priority|The priority of an index in the cold phase.|

