# Quick start

This topic describes how to use developer tools, such as Alibaba Cloud CLI, OpenAPI Explorer, and Alibaba Cloud SDKs, to call an Elasticsearch API operation. This topic uses the ListSearchLog operation as an example.

Before you call an API operation, you must understand related instructions in [API documentation](/intl.en-US/API Reference/List of operations by function.md) and obtain the values of the required request parameters. If an error is reported after you send a request, you can obtain the description of the error code in API documentation.

## Methods to make API requests

-   [Alibaba Cloud CLI](#section_mzy_n7e_lqt)

    If you want to use a command line tool, you can use this method. Before you use this method, make sure that Alibaba Cloud CLI is installed on the host that you use to send requests. For more information about how to install Alibaba Cloud CLI in different operating systems, see the following topics:

    -   [Windows]()
    -   [Linux]()
    -   [MacOS]()
-   [OpenAPI Explorer](#section_nmi_h5u_lal)

    You can use this method if you want to use an interactive operation interface or you are a developer user who is unfamiliar with Alibaba Cloud products. You can also debug API operations and obtain sample SDK requests in OpenAPI Explorer. For more information, see [What is OpenAPI Explorer?](/intl.en-US/Product Overview/What is OpenAPI Explorer?.md)

-   [SDK for Java](#section_psi_p9e_1vv)

    SDK for Java is ideal for scenarios such as SDK encoding or DevOps. Before you use SDK for Java, you must install a [JDK](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html) and [Maven](http://maven.apache.org/download.cgi?spm=a2c4g.11186623.2.10.31a073fbOAanmH&file=download.cgi). The JDK version must be 1.6 or later.


## Alibaba Cloud CLI

1.  Use one of the following methods to obtain the ID of the cluster you want to access:

    -   In the Kibana console, run the `GET /` command. For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).
    -   On your on-premises machine, call the ListInstance operation.

        ```
        aliyun elasticsearch ListInstance --zoneId cn-hangzhou
        ```

2.  Call the ListSearchLog operation to query the system logs of the cluster.

    ```
    aliyun elasticsearch ListSearchLog --type INSTANCELOG --query level:warn --beginTime 1593332477000 --endTime 1593418877000 --page 1 --size 20 --InstanceId es-cn-n6w1o1x0w00******   
    ```


## OpenAPI Explorer

1.  Call the [ListInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/ListInstance.md) operation to obtain the ID of the cluster you want to access.

2.  Call the [ListSearchLog](/intl.en-US/API Reference/Elasticsearch instances/Query logs/ListSearchLog.md) operation to query the system logs of the cluster.

    If the query is successful, OpenAPI Explorer displays related logs.


## SDK for Java

1.  Create a Maven project.

    **Note:** For more information, see [Install ECS SDK for Java](/intl.en-US/SDK Reference/Java example/Install ECS SDK for Java.md).

2.  Add POM dependencies.

    ```
     <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-elasticsearch</artifactId>
            <version>3.0.16</version>
      </dependency>
      <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-core</artifactId>
            <version>4.4.6</version>
       </dependency>
    ```

3.  Create a Java program. Then, call the ListSearchLog operation to query the system logs of the cluster that you want to access.

    ```
    import com.aliyuncs.CommonRequest;
    import com.aliyuncs.CommonResponse;
    import com.aliyuncs.DefaultAcsClient;
    import com.aliyuncs.IAcsClient;
    import com.aliyuncs.exceptions.ClientException;
    import com.aliyuncs.exceptions.ServerException;
    import com.aliyuncs.http.FormatType;
    import com.aliyuncs.http.MethodType;
    import com.aliyuncs.profile.DefaultProfile;
    
    public class ListSearchLog {
        public static void main(String[] args) {
            DefaultProfile profile = DefaultProfile.getProfile("<RegionId>", "<accessKeyId>", "<accessSecret>");
            IAcsClient client = new DefaultAcsClient(profile);
    
            CommonRequest request = new CommonRequest();
            //request.setProtocol(ProtocolType.HTTPS);
            request.setMethod(MethodType.GET);
            request.setDomain("elasticsearch. <RegionId>.aliyuncs.com");
            request.setVersion("2017-06-13");
            request.setUriPattern("/openapi/instances/<instanceid>/search-log");
            request.putQueryParameter("type", "INSTANCELOG");
            request.putQueryParameter("query", "level:warn");
            request.putQueryParameter("beginTime", "1593332477000");
            request.putQueryParameter("endTime", "1593418877000");
            request.putQueryParameter("page", "1");
            request.putQueryParameter("size", "20");
            request.putHeadParameter("Content-Type", "application/json");
            String requestBody = "" +
                    "{}";
            request.setHttpContent(requestBody.getBytes(), "utf-8", FormatType.JSON);
            try {
                CommonResponse response = client.getCommonResponse(request);
                System.out.println(response.getData());
            } catch (ServerException e) {
                e.printStackTrace();
            } catch (ClientException e) {
                e.printStackTrace();
            }
        }
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |<RegionId\>|The ID of the region where the cluster resides. For more information about how to obtain the ID, see [t134305.md\#section\_ydy\_ply\_zgb](/intl.en-US/RAM/Types of resources that can be authorized.md).|
    |<accessKeyId\>|The AccessKey ID of your Alibaba Cloud account. For more information about how to obtain the AccessKey ID, see [Create an AccessKey pair]().|
    |<accessSecret\>|The AccessKey secret of your Alibaba Cloud account. For more information about how to obtain the AccessKey secret, see [Create an AccessKey pair]().|
    |<instanceId\>|The ID of the cluster. For more information about how to obtain the ID, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) and [ListInstance](/intl.en-US/API Reference/Elasticsearch instances/Manage clusters/ListInstance.md).|

    **Note:** For more information about request parameters, see [ListSearchLog](/intl.en-US/API Reference/Elasticsearch instances/Query logs/ListSearchLog.md).


