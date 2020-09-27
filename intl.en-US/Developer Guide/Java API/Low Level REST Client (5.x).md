# Low Level REST Client \(5.x\)

This topic describes how to call Elasticsearch Java API operations. Java High Level REST Client 5.x is used as an example.

## Precautions

-   Low Level REST Client 5.x is compatible only with Alibaba Cloud Elasticsearch V5.5.3. For more information about how to use a Java REST client to access an Elasticsearch V6.3.2 cluster, see [Java REST Client 6.3.2](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.3/index.html).
-   The version of the Java REST client must be the same as that of your Elasticsearch cluster.

## Preparations

-   Install a JDK. The JDK version must be 1.8 or later.

    For more information, see [Install the JDK](/intl.en-US/Best Practices/Migrate and synchronize MySQL data/RDS synchronization/Use Canal to synchronize data to an Alibaba Cloud Elasticsearch cluster.md).

-   Create an Alibaba Cloud Elasticsearch V5.5.3 cluster.

    For more information, see [Create an Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Elasticsearch cluster.md).

-   Enable the Auto Indexing feature for the Elasticsearch cluster.

    For more information, see [Enable auto indexing](/intl.en-US/Quick Start/Step 2 (optional): Configure a cluster.md).

    If the Auto Indexing feature is not enabled, the following error is reported.

    ![Error](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5487649951/p97345.png)

-   Configure a whitelist for the Elasticsearch cluster to ensure normal communication among networks.
    -   If the server that runs Java code is located in an Internet environment, you can access the Elasticsearch cluster by using its public endpoint. Before you access the cluster, you must enable the Public Network Access feature for the cluster and add the public IP address of the server to the Internet whitelist of the cluster. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).

        **Note:**

        -   If you are using a public network, add the IP address of the jump server that controls outbound traffic of the public network to the whitelist.
        -   You can also add 0.0.0.0/0 to the whitelist to allow requests from all IPv4 addresses. If you make this configuration, all public IP addresses can be used to access the Elasticsearch cluster. This poses security risks. We recommend that you evaluate the risks before you make this configuration.
    -   If the server that runs Java code is located in the same Virtual Private Cloud \(VPC\) as the Elasticsearch cluster, you can access the cluster by using its internal endpoint. Before you access the cluster, make sure that the internal IP address of the server is added to the VPC whitelist of the cluster. By default, 0.0.0.0/0 is added to the whitelist.
-   Create a Java Maven project and add the following [Project Object Model \(POM\) dependencies](#section_pi7_1uv_yit) to the pom.xml file of the Java project.

## POM dependencies

```
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>rest</artifactId>
    <version>5.5.3</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.7</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.7</version>
</dependency>
```

## Example

You can click [here](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/33813/cn_zh/1593402410101/es5.5-demo.zip) to download complete sample code.

In the following sample code, the Java REST client is connected to the Elasticsearch cluster over port 9200.

```
import org.apache.http.HttpEntity;
import org.apache.http.HttpHost;
import org.apache.http.auth.AuthScope;
import org.apache.http.auth.UsernamePasswordCredentials;
import org.apache.http.client.CredentialsProvider;
import org.apache.http.entity.ContentType;
import org.apache.http.impl.client.BasicCredentialsProvider;
import org.apache.http.impl.nio.client.HttpAsyncClientBuilder;
import org.apache.http.nio.entity.NStringEntity;
import org.apache.http.util.EntityUtils;
import org.elasticsearch.client.Response;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestClientBuilder;
import java.io.IOException;
import java.util.Collections;
public class RestClientTest55 {
    public  static void main(String[]args){
        final CredentialsProvider credentialsProvider = new BasicCredentialsProvider();
        credentialsProvider.setCredentials(AuthScope.ANY,
                new UsernamePasswordCredentials("USER NAME", "PASSWORD"));
        // Set HOST to the public endpoint of the Elasticsearch cluster. You can obtain the endpoint from the Basic Information page of the cluster.
        RestClient restClient = RestClient.builder(new HttpHost("HOST", 9200))
                .setHttpClientConfigCallback(new RestClientBuilder.HttpClientConfigCallback() {
                    @Override
                    public HttpAsyncClientBuilder customizeHttpClient(HttpAsyncClientBuilder httpClientBuilder) {
                        return httpClientBuilder.setDefaultCredentialsProvider(credentialsProvider);
                    }
                }).build();
        try {
            // field_01 and field_02 are field names, and value_01 and value_02 are the values of field_01 and field_02.
            HttpEntity entity = new NStringEntity("{\n\"field_01\" : \"value_01\"\n,\n\"field_02\" : \"value_02\"\n}", ContentType.APPLICATION_JSON);
            // index_name is the index name, type_name is the type name, and doc_id is the document ID.
            Response indexResponse = restClient.performRequest(
                    "PUT",
                    "/index_name/type_name/doc_id",
                    Collections. <String, String>emptyMap(),
                    entity);
            // index_name is the index name, type_name is the type name, and doc_id is the document ID. The index name, type name, and document ID are the same as those you specified when you create the index.
            Response response = restClient.performRequest("GET", "/index_name/type_name/doc_id",
                    Collections.singletonMap("pretty", "true"));
            System.out.println(EntityUtils.toString(response.getEntity()));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

|Parameter|Description|
|---------|-----------|
|`USER NAME`|Replace it with the username of the Elasticsearch cluster.|
|`PASSWORD`|Replace it with the password of the Elasticsearch cluster.|
|`HOST`|Replace it with the public or internal endpoint of the Elasticsearch cluster. You can obtain the endpoint from the Basic Information page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|

