# ListTags

Call ListTags to query all visible tags of a user.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ListTags&type=ROA&version=2017-06-13)

## Request header

This operation uses only common request headers. For more information, see the Common request parameters topic.

## Request syntax

```
GET /openapi/tags/all-tags HTTPS|HTTP
```

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|pageSize|Integer|Yes|20|The number of returned pages. |
|resourceType|String|No|INSTANCE|The type of the resource. Set the value to INSTANCE. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|5FFD9ED4-C2EC-4E89-B22B-1ACB6FE1D\*\*\*|The ID of the request. |
|Result|Array of Result| |The return results. |
|TagKey|String|env|The key of the tag. |
|TagValue|String|dev|The value of the tag. |

## Examples

Sample requests

```
GET /openapi/tags/all-tags? pageSize=20 HTTP/1.1
Common request header
```

Sample success responses

`XML` format

```
<Result>
    <TagKey>aa</TagKey>
    <TagValue>ddd</TagValue>
</Result>
<Result>
    <TagKey>manager</TagKey>
    <TagValue>all_persion</TagValue>
</Result>
<Result>
    <TagKey>dev</TagKey>
    <TagValue>leizhang</TagValue>
</Result>
<Result>
    <TagKey>a</TagKey>
    <TagValue>b</TagValue>
</Result>
<Result>
    <TagKey>zl</TagKey>
    <TagValue>keept</TagValue>
</Result>
<Result>
    <TagKey>zl</TagKey>
    <TagValue/>
</Result>
<Result>
    <TagKey>c</TagKey>
    <TagValue>d</TagValue>
</Result>
<Result>
    <TagKey>tt</TagKey>
    <TagValue>tt</TagValue>
</Result>
<Result>
    <TagKey>acs:rm:rgId</TagKey>
    <TagValue>rg-acfm2h5vbzd****</TagValue>
</Result>
<Result>
    <TagKey>acs:rm:rgId</TagKey>
    <TagValue>rg-aek22gedcbf****</TagValue>
</Result>
<Result>
    <TagKey>estag</TagKey>
    <TagValue>instance</TagValue>
</Result>
<RequestId>5ADCBB89-6596-4AF3-94A5-64E5393A****</RequestId>
```

`JSON` format

```
{
    "Result": [
        {
            "TagKey": "aa",
            "TagValue": "ddd"
        },
        {
            "TagKey": "manager",
            "TagValue": "all_persion"
        },
        {
            "TagKey": "dev",
            "TagValue": "leizhang"
        },
        {
            "TagKey": "a",
            "TagValue": "b"
        },
        {
            "TagKey": "zl",
            "TagValue": "keept"
        },
        {
            "TagKey": "zl",
            "TagValue": ""
        },
        {
            "TagKey": "c",
            "TagValue": "d"
        },
        {
            "TagKey": "tt",
            "TagValue": "tt"
        },
        {
            "TagKey": "acs:rm:rgId",
            "TagValue": "rg-acfm2h5vbzd****"
        },
        {
            "TagKey": "acs:rm:rgId",
            "TagValue": "rg-aek22gedcbf****"
        },
        {
            "TagKey": "estag",
            "TagValue": "instance"
        }
    ],
    "RequestId": "5ADCBB89-6596-4AF3-94A5-64E5393A****"
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch).

