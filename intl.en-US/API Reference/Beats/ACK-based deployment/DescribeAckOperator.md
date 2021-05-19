# DescribeAckOperator

Call the DescribeAckOperator to view the Elasticsearch Operator information installed on the specified Alibaba Cloud Container Service for Kubernetes ACK\(Container Service for Kubernetes\) cluster.

**Note:** Before installing the collector on the ACK cluster, you can call this interface to view the installation status of the Elasticsearch Operator on the target cluster.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. After you call an operation, OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DescribeAckOperator&type=ROA&version=2017-06-13)

## Request header

This operation uses only the common request header. For more information, see Common request parameters.

## Request structure

```

     GET /openapi/ack-clusters/[ClusterId]/operator HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|ClusterId|String|Path|Yes|c79acd3fbf462423fb6450e513bb6\*\*\*\*|The ID of the cluster from which you want to detach tags. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|6615EE8D-FD9D-4FD3-997E-6FEA5B8D82ED|The ID of the request. |
|Result|Struct| |The returned results. |
|status|String|deployed|The operator installation status. Then, you can perform the following operations:

-   deployed: installed
-   not-deploy: not installed
-   failed: installation failed
-   unknown: unknown status |
|version|String|1|The Operator version. |

## Examples

Sample requests

```

     GET /openapi/ack-clusters/c79acd3fbf462423fb6450e513bb6 ****/operator HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": { "version": "1", "status": "deployed" }, "RequestId": "6615EE8D-FD9D-4FD3-997E-6FEA5B8D82ED" } 
   
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

