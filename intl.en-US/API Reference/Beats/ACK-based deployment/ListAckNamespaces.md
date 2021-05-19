# ListAckNamespaces

Call the ListAckNamespaces to view all namespaces of the specified Alibaba Cloud Container Service for Kubernetes ACK\(Container Service for Kubernetes\) cluster.

**Note:** When you create an ACK cluster-based collector, you need to specify the namespace of the cluster. You can call this interface to view all namespaces of the cluster and select the appropriate namespace based on this.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. After you call an operation, OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListAckNamespaces&type=ROA&version=2017-06-13)

## Request header

This operation uses only the common request header. For more information, see Common request parameters.

## Request structure

```

     GET /openapi/ack-clusters/[ClusterId]/namespaces HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|ClusterId|String|Path|Yes|c79acd3fbf462423fb6450e513bb6\*\*\*\*|The ID of the cluster from which you want to detach tags. |
|page|Integer|Query|No|1|Set the number of returned result pages. |
|size|Integer|Query|No|10|The number of records contained per page. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|95789100-A329-473B-9D14-9E0B7DB4BD5A|The ID of the request. |
|Result|Array of Result| |The return results. |
|namespace|String|logging|The namespace of the cluster. |
|status|String|Active|The namespace status. |

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

|5

|The number of returned records. |

**Note:** └ indicates a child parameter.

## Examples

Sample requests

```

     GET /openapi/ack-clusters/c79acd3fbf462423fb6450e513bb6 ****/namespaces HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "namespace": "arms-prom", "status": "Active" }, { "namespace": "default", "status": "Active" }, { "namespace": "kube-node-lease", "status": "Active" }, { "namespace": "kube-public", "status": "Active" }, { "namespace": "kube-system", "status": "Active" }, { "namespace": "logging", "status": "Active" } ], "RequestId": "95789100-A329-473B-9D14-9E0B7DB4BD5A", "Headers": { "X-Total-Count": 6 } } 
   
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

