# ListTags

Queries all the visible user tags.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListTags&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/tags/all-tags HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|pageSize|Integer|Query|No|20|The number of pages of the returned result. Default value: 50. |
|resourceType|String|Query|No|INSTANCE|The resource type, which is fixed to INSTANCE. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|TagKey|String|env|The tag key of the ENI. |
|TagValue|String|dev|The tag value of the resource. |

## Examples

Sample requests

```

     GET /openapi/tags/all-tags?pageSize=20 HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "Result": [ { "TagKey": "aa", "TagValue": "ddd" }, { "TagKey": "manager", "TagValue": "all_persion" }, { "TagKey": "dev", "TagValue": "leizhang" }, { "TagKey": "a", "TagValue": "b" }, { "TagKey": "zl", "TagValue": "keept" }, { "TagKey": "zl", "TagValue": "" }, { "TagKey": "c", "TagValue": "d" }, { "TagKey": "tt", "TagValue": "tt" }, { "TagKey": "acs:rm:rgId", "TagValue": "rg-acfm2h5vbzd****" }, { "TagKey": "acs:rm:rgId", "TagValue": "rg-aek22gedcbf****" }, { "TagKey": "estag", "TagValue": "instance" } ], "RequestId": "5ADCBB89-6596-4AF3-94A5-64E5393A****" } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

