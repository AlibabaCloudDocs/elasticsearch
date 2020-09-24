# Request signatures

You must sign all API requests to ensure security. Alibaba Cloud uses the request signature to verify the identity of the API caller. Each API request must contain the signature, regardless of whether it is sent by using HTTP or HTTPS.

## Overview

To sign a RESTful API request, you must add the Authorization parameter to the request header in the following format:

```
Authorization:acs:AccessKeyId:Singature
```

-   acs: the abbreviation for Alibaba Cloud Service. This is a fixed field and cannot be modified.
-   AccessKeyId: the AccessKey ID that is used to call the API.
-   Signature: the signature generated after the request is symmetrically encrypted by using the AccessKey secret.

## Signature calculation

The signature algorithm complies with the HMAC-SHA1 specifications in RFC 2104. It uses an AccessKey secret to calculate the HMAC value of an encoded, formatted query string and use the HMAC value as the signature. Some parameters in a request are used to calculate the signature. Therefore, the signatures of requests vary with API request parameters.

```
Signature = Base64( HMAC-SHA1( AccessSecret, UTF-8-Encoding-Of(
StringToSign)) )
```

To calculate a signature, perform the following steps:

1.  Compose a string-to-sign.

    The string-to-sign is a string assembled by using related elements in an API request. It is used to calculate the signature and contains the following elements:

    -   HTTP header
    -   Alibaba Cloud protocol headers \(CanonicalizedHeaders\)
    -   Canonicalized resource \(CanonicalizedResource\)
    -   Body
    The string-to-sign must be created in the following format:

    ```
    StringToSign = 
           //HTTP header
            HTTP-Verb + "\n" +
            Accept + "\n" +
            Content-MD5 + "\n" +//Specify the request body that is encrypted by using the MD5 algorithm.
            Content-Type + "\n" +
            Date + "\n" +
           //Alibaba Cloud protocol headers (CanonicalizedHeaders)
            CanonicalizedHeaders +
           //Canonicalized resource (CanonicalizedResource)
            CanonicalizedResource
    ```

    Sample original request:

    ```
    POST /stacks?name=test_alert&status=COMPLETE HTTP/1.1
    Host: ***.aliyuncs.com
    Accept: application/json
    Content-MD5: ChDfdfwC+Tn874znq7Dw7Q==
    Content-Type: application/x-www-form-urlencoded;charset=utf-8
    Date: Thu, 22 Feb 2018 07:46:12 GMT 
    x-acs-signature-nonce: 550e8400-e29b-41d4-a716-446655440000
    x-acs-signature-method: HMAC-SHA1
    x-acs-signature-version: 1.0
    x-acs-version: 2016-01-02
    ```

    Sample canonicalized request:

    ```
    POST
    application/json
    ChDfdfwC+Tn874znq7Dw7Q==
    application/x-www-form-urlencoded;charset=utf-8
    Thu, 22 Feb 2018 07:46:12 GMT
    x-acs-signature-nonce: 550e8400-e29b-41d4-a716-446655440000
    x-acs-signature-method:HMAC-SHA1
    x-acs-signature-version:1.0
    x-acs-version:2016-01-02
    /stacks?name=test_alert&status=COMPLETE
    ```

2.  Add the signature string.

    Add the calculated signature string to the request header in the following format:

    ```
    Authorization: acs AccessKeyId:Signature
    ```


## HTTP header

The HTTP header in the string-to-sign must contain the values of the following parameters. The parameters must be sorted in alphabetical order. If a parameter has no value, set it to `\n`.

-   Accept: the response type required by the client. Valid values: application/json and application/xml.
-   Content-MD5: the Base64-encoded 128-bit MD5 hash value of the request body.
-   Content-Type: the type of the HTTP request body. The type is defined in RFC 2616.
-   Date: the GMT time specified in HTTP 1.1. Example: Wed, 05 Sep.2012 23:00:00 GMT.

    **Note:** You do not need to specify your AccessKey pair in the HTTP header.


Sample original header:

```
Accept: application/json
Content-MD5: ChDfdfwC+Tn874znq7Dw7Q==
Content-Type: application/x-www-form-urlencoded;charset=utf-8
Date: Thu, 22 Feb 2018 07:46:12 GMT
```

Sample canonicalized header:

```
application/json
ChDfdfwC+Tn874znq7Dw7Q==
application/x-www-form-urlencoded;charset=utf-8
Thu, 22 Feb 2018 07:46:12 GMT
```

## Alibaba Cloud protocol headers \(CanonicalizedHeaders\)

CanonicalizedHeaders are non-standard HTTP headers of Alibaba Cloud. They are parameters prefixed with `x-acs-` in a request. A request must contain the following parameters:

-   x-acs-signature-nonce: a unique, random number used to prevent replay attacks. You must use different numbers for different requests.
-   x-acs-signature-version: the version of the signature encryption algorithm. Set the value to 1.0.
-   x-acs-version: the version number of the API.

To construct Alibaba Cloud canonicalized headers, perform the following steps:

1.  Convert the names of all HTTP request headers prefixed with `x-acs-` to lowercase letters. For example, convert `X-acs-OSS-Meta-Name: TaoBao` into `x-acs-oss-meta-name: TaoBao`.
2.  Arrange all HTTP request headers that are obtained from the preceding step in alphabetical order.
3.  Delete spaces from each side of a delimiter between the request header and its content. For example, convert `x-acs-oss-meta-name: TaoBao,Alipay` into `x-acs-oss-meta-name:TaoBao,Alipay`.
4.  Separate all headers and content with delimiters \(\\n\) to form the final CanonicalizedHeaders.

Sample original header:

```
x-acs-signature-nonce: 550e8400-e29b-41d4-a716-446655440000
x-acs-signature-method: HMAC-SHA1
x-acs-signature-version: 1.0
x-acs-version: 2016-01-02GMT
```

Sample canonicalized header:

```
x-acs-signature-nonce:550e8400-e29b-41d4-a716-446655440000
x-acs-signature-method:HMAC-SHA1
x-acs-signature-version:1.0
x-acs-version:2016-01-02
```

## Canonicalized resource \(CanonicalizedResource\)

CanonicalizedResource is the canonical description of the resource that you want to access. Sort sub-resources along with the query parameters in ascending lexicographic order and separate them with ampersands \(&\) to generate a sub-resource string. The sub-resource string consists of all parameters that follow the question mark \(?\).

Sample original request:

```
/stacks?status=COMPLETE&name=test_alert
```

Sample canonicalized request:

```
/stacks?name=test_alert&status=COMPLETE
```

## Body

Encrypt the request body by using the MD5 algorithm, encode the encrypted request body in Base64, and add the Base64-encoded string to the value of the Content-MD5 parameter.

