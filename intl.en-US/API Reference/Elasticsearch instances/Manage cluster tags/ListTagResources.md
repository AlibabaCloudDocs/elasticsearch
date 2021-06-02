# ListTagResources

Queries the tags that are added to one or more resources.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListTagResources&type=ROA&version=2017-06-13)

## Request headers

This operation uses only the common request header. For more information, see Common request parameters.

## Request syntax

```

     GET /openapi/tags HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|ResourceType|String|Query|Yes|INSTANCE|The type of the resource. |
|Page|Integer|Query|No|1|The number of pages in the resource relationship list. |
|NextToken|String|Query|No|1d2db86sca4384811e0b5e8707e\*\*\*\*\*\*|The token that was returned for the next query. |
|Size|Integer|Query|No|10|The number of entries to return on each page. |
|ResourceIds|String|Query|No|\["es-cn-aaa","es-cn-bbb"\]|The list of instance IDs to query. |
|Tags|String|Query|No|\[\{"key":"env","value","dev"\},\{"key":"dev", "value":"IT"\}\]|The list of Tags to be queried, in the form of a JSON string, contains up to 20 subitems. |

-   You can only **ResourceIds** and **Tags** Parameter, select at least one incoming value, otherwise an error is reported.
-   You can only query visible tags, not invisible tags.

**Note:** System tags refer to the tags added by cloud products \(that is, Alibaba Cloud services\) to user instances. System tags are divided into visible tags and invisible tags.


## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. This parameter is empty and is for reference only. This parameter cannot be forcibly relied on in the program.

**Note:** This parameter is not included in the return example. |
|X-Total-Count|Integer|10|The number of resources that are queried to TagResource. |
|PageSize|Integer|1|The number of the returned page. |
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6D\*\*\*\*\*\*|The ID of the request. |
|TagResources|Struct| |The group of tags and resources. |
|TagResource|Array of TagResource| |The tags returned for the specified resource. |
|ResourceId|String|es-cn-oew1q8bev0002\*\*\*\*|The ID of the resource. |
|ResourceType|String|ALIYUN::ELASTICSEARCH::INSTANCE|The resource type. Fixed as `ALIYUN::ELASTICSEARCH::INSTANCE` . |
|TagKey|String|env|The tag key of the ENI. |
|TagValue|String|dev|The tag value of the resource. |

## Examples

Sample requests

```

     GET /openapi/tags?ResourceType=INSTANCE HTTP/1.1 public request header 
   
```

Sample success responses

`JSON` format

```

     { "RequestId": "A5423B74-DBD5-4714-BF28-6725E5BE****", "TagResources": { "TagResource": [ { "ResourceType": "ALIYUN::ELASTICSEARCH::INSTANCE", "ResourceId": "es-cn-oew1q8bev0002****", "TagKey": "env", "TagValue": "dev" } ] } } 
   
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

