# ModifyDeployMachine

Call the ModifyDeployMachine to update the ECS machine installed by the collector.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. After you call an operation, OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=elasticsearch&api=ModifyDeployMachine&type=ROA&version=2017-06-13)

## Request header

This operation uses only the common request header. For more information, see Common request parameters.

## Request structure

```

     POST /openapi/collectors/[ResId]/actions/modify-deploy-machines HTTP/1.1 
   
```

## Request parameters

|Parameter|Type|Position|Required|Example|Description|
|---------|----|--------|--------|-------|-----------|
|ResId|String|Path|Yes|ct-cn-xb1i7q79u65nk\*\*\*\*|The collector ID. |
|ClientToken|String|Query|No|5A2CFF0E-5718-45B5-9D4D-70B3FF\*\*\*\*|This parameter is used to ensure the idempotence of the request. The value of this parameter is generated by the client and is unique among different requests. The maximum length is 64 ASCII characters. |

## RequestBody

The following parameters must be filled in the RequestBody to specify the information of the target ECS instance.

|Parameter

|Type

|Required

|Example

|Description |
|-----------|------|----------|---------|-------------|
|machines

|List

|Yes

| |The target ECS instance information. |
|└instanceId

|String

|Yes

|i-bp11u91xgubypcuz\*\*\*\*

|The ID of the instance. |
|type

|String

|Yes

|ECSInstanceId

|The type of machine that the collector is deployed. Only ECSInstanceId\(ECS machine deployment\) is supported. |
|configType

|String

|Yes

|collectorDeployMachine

|The type of the configuration. Only collectorDeployMachine \(the deployment machine of the collector\) is supported. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|C37CE536-6C0F-4778-9B59-6D94C7F7EB63|The ID of the request. |
|Result|Boolean|true|Whether the update succeeded:

-   true: The operation was successful.
-   false: The operation failed. |

## Examples

Sample requests

```

     POST /openapi/collectors/ct-cn-xb1i7q79u65nk **** /actions/modify-deploy-machines HTTP/1.1 public request header {"machines":[ {"instanceId":"i-bp1ei8ysh7orb6eq ****"}, {"instanceId":"i-bp12plyjhrv7eobp ****"} ], "type":"ECSInstanceId", "configType":"collectorDeployMachine"} 
   
```

Sample success responses

`JSON` format

```

     { "Result": true, "RequestId": "C37CE536-6C0F-4778-9B59-6D94C7F7EB63" } 
   
```

## Error codes

Visit the [Error Center](https://error-center.alibabacloud.com/status/product/elasticsearch)View more error codes.
