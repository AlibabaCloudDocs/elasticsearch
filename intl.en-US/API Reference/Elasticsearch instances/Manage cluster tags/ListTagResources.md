# ListTagResources

Call ListTagResources to query tags that are attached to one or more resources.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListTagResources&type=ROA&version=2017-06-13)

## Request header

This operation uses common request parameters only. For more information, see Common parameters.

## Request syntax

```
GET /openapi/tags HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|ResourceType|String|Yes|INSTANCE|The type of the resource. |
|Page|Integer|No|1|The number of entries to return on each page. |
|NextToken|String|No|1d2db86sca4384811e0b5e8707e\*\*\*\*\*\*|The token that is returned for the next query. |
|Size|Integer|No|10|The number of entries to return on each page. |
|ResourceIds|String|No|\["es-cn-aaa","es-cn-bbb"\]|The list of instance IDs that you want to query. |
|Tags|String|No|\[\{"key":"env","value","dev"\},\{"key":"dev", "value":"IT"\}\]|The list of Tags to be queried. It is a JSON string that contains up to 20 subitems. |

-   You can only select an input value for the **ResourceIds** and **Tags** parameters. Otherwise, an error is returned.
-   You can only query visible tags. You cannot query invisible tags.

**Note:** System labels are added to instances by Alibaba Cloud services. System labels are divided into visible labels and invisible labels.


## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Headers|Struct| |The header of the response. The value of this parameter is empty. The parameter is for reference only. The program is not strongly dependent on this parameter.

**Note:** The response does not contain this parameter. |
|X-Total-Count|Integer|10|The number of queried TagResource. |
|PageSize|Integer|1|The page number of the returned page. |
|RequestId|String|F99407AB-2FA9-489E-A259-40CF6D\*\*\*\*\*\*|The ID of the request. |
|TagResources|Struct| |The group of tags and resources. |
|TagResource|Array of TagResource| |The tags returned for the specified resource. |
|ResourceId|String|es-cn-oew1q8bev0002\*\*\*\*|The ID of the resource. |
|ResourceType|String|ALIYUN::ELASTICSEARCH::INSTANCE|The type of the resource. Fixed to `ALIYUN::ELASTICSEARCH instances` |
|TagKey|String|env|The tag key of the resource. |
|TagValue|String|dev|The tag value of the resource. |

## Examples

Sample requests

```
GET /openapi/tags? ResourceType=INSTANCE HTTP/1.1
Common request header
```

Sample success responses

`XML` format

```
<RequestId>A5423B74-DBD5-4714-BF28-6725E5BE****</RequestId>
<TagResources>
    <TagResource>
        <ResourceType>ALIYUN::ELASTICSEARCH::INSTANCE</ResourceType>
        <ResourceId>es-cn-oew1q8bev0002****</ResourceId>
        <TagKey>env</TagKey>
        <TagValue>dev</TagValue>
    </TagResource>
</TagResources>
```

`JSON` format

```
{
    "RequestId": "A5423B74-DBD5-4714-BF28-6725E5BE****",
    "TagResources": {
        "TagResource": [
            {
                "ResourceType": "ALIYUN::ELASTICSEARCH::INSTANCE",
                "ResourceId": "es-cn-oew1q8bev0002****",
                "TagKey": "env",
                "TagValue": "dev"
            }
        ]
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

