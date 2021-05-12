# ListAckClusters

Call the ListAckClusters to obtain the list of Alibaba Cloud Container Service for Kubernetes ACK\(Container Service for Kubernetes\) clusters.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. After you call an operation, OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListAckClusters&type=ROA&version=2017-06-13)

## Request header

This operation uses only the common request header. For more information, see Common request parameters.

## Request structure

```

     GET /openapi/ack-clusters HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|page|Integer|Query|No|3|Set the number of pages for the returned result. |
|size|Integer|Query|No|20|The number of results that each page contains. |
|vpcId|String|Query|No|vpc-bp12nu14urf0upaf4\*\*\*\*|The Virtual Private Cloud ID. where the ACK cluster resides |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|F93EAA49-284F-4FCE-9E67-FA23FB4BB512|The ID of the request. |
|Result|Array of result| |The return results. |
|clusterId|String|c5ea2c2d9a3cf499481292f60425d\*\*\*\*|The ID of the cluster. |
|clusterType|String|ManagedKubernetes|Cluster type, which supports only ManagedKubernetes, that is, Kubernetes clusters. |
|name|String|test|The name of the cluster. |
|vpcId|String|vpc-bp12nu14urf0upaf4\*\*\*\*|The ID of the VPC where the source cluster resides. |

The following parameters are also included in the returned data.

|Parameter

|Type

|Example

|Description |
|-----------|------|---------|-------------|
|Headers

|Struct

| |The header of the response. |
|└X-Total-Count

|Integer

|2

|The number of returned records. |

**Note:** └ indicates a child parameter.

## Examples

Sample requests

```

     GET /openapi/ack-clusters?Page=1&size=20 HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "clusterId": "c5ea2c2d9a3cf499481292f60425d****", "name": "test", "clusterType": "ManagedKubernetes", "vpcId": "vpc-bp12nu14urf0upaf4****" }, { "clusterId": "cdcee8be4e87a40e2a23fdbc1c24d****", "name": "cs", "clusterType": "ManagedKubernetes", "vpcId": "vpc-bp16k1dvzxtmagcva****" } ], "RequestId": "F93EAA49-284F-4FCE-9E67-FA23FB4BB512", "Headers": { "X-Total-Count": 2 } } 
   
```

## Error codes

Visit the [Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch)View more error codes.

