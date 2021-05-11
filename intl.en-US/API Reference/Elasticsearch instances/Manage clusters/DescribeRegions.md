# DescribeRegions

Call DescribeRegions to obtain the region information of an Alibaba Cloud Elasticsearch.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=DescribeRegions&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/regions HTTPS|HTTP   
```

## Request parameters

None

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Array of regionInfo| |The list of returned results. |
|consoleEndpoint|String|https://elasticsearch-cn-hangzhou.console.aliyun.com|The Endpoint exposed by the console in this region. |
|localName|String|China \(Hangzhou\)|The name of the region where a server resides. |
|regionEndpoint|String|elasticsearch.cn-hangzhou.aliyuncs.com|The Endpoint of the region. |
|regionId|String|cn-hangzhou|The ID of the region to which the cluster belongs. |
|status|String|available|The availability status of the region. |

## Examples

Sample requests

```
GET /openapi/regions?RegionId=cn-hangzhou HTTP/1.1 
common request header  
```

Sample success responses

`JSON` Format

```
{
    "Result": [
          {
            "regionId": "cn-hangzhou",
            "localName": "China (Hangzhou)",
            "regionEndpoint": "elasticsearch.cn-hangzhou.aliyuncs.com",
            "consoleEndpoint": "https://elasticsearch-cn-hangzhou.console.aliyun.com",
            "status": "available"
          }
        ]
}
```

## Error code

Go to the [Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch). For more information, see error codes.

