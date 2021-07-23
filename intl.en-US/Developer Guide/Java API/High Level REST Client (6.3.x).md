# High Level REST Client \(6.3.x\)

This topic describes how to call Elasticsearch Java API operations. Java High Level REST Client 6.3.x is used as an example.

## Preparations

-   Install a JDK. The JDK version must be 1.8 or later.

    For more information, see [Install a JDK](/intl.en-US/Best Practices/Migrate and synchronize MySQL data/RDS synchronization/Use Canal to synchronize MySQL data to Alibaba Cloud Elasticsearch.md).

-   Create an Alibaba Cloud Elasticsearch V6.3.2 cluster. Make sure that the cluster version is later than or the same as the version of Java High Level REST Client you use.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

    **Note:** Java High Level REST Client is forward compatible. For example, Java High Level REST Client 6.3.2 can communicate with Elasticsearch clusters of V6.3.2 or later. To ensure that you can use the features of the latest client, we recommend that the version of Java High Level REST Client you use is the same as that of your cluster.

-   Enable the Auto Indexing feature for the cluster.

    For more information, see [Configure the YML file](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md).

    If the Auto Indexing feature is not enabled, the following error is reported.

    ![Error](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5487649951/p97345.png)

-   Configure a whitelist for the cluster to ensure normal communication among networks.
    -   If the server that runs Java code is located in an Internet environment, you can access the cluster by using its public endpoint. Before you access the cluster, you must enable the Public Network Access feature for the cluster and add the public IP address of the server to the public IP address whitelist of the cluster. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).

        **Note:**

        -   If your client is in a home network or in a LAN of an office, add the IP address of the Internet egress to the whitelist rather than the internal mechanism of the client.
        -   You can also add 0.0.0.0/0 to the whitelist to allow requests from all IPv4 addresses. If you make this configuration, all public IP addresses can be used to access the cluster. This poses security risks. We recommend that you evaluate the risks before you make this configuration.
    -   If the server that runs Java code is located in the same virtual private cloud \(VPC\) as the cluster, you can access the cluster by using its internal endpoint. Before you access the cluster, make sure that the private IP address of the server is added to the private IP address whitelist of the cluster. By default, 0.0.0.0/0 is added to the whitelist.
-   Create a Java Maven project and add the following [Project Object Model \(POM\) dependencies](#section_flr_0uz_6lx) to the pom.xml file of the Java project.

## POM dependencies

```
<dependency>
    <groupId>org.elasticsearch</groupId>
    <artifactId>elasticsearch</artifactId>
    <version>6.3.2</version>
</dependency>
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>6.3.2</version>
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

You can download the [complete sample code](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/33813/cn_zh/1593402288793/es6.3-demo.zip).

The following sample code calls the index API operation to create an index and calls the delete API operation to delete the index:

```
import org.apache.http.HttpHost;
import org.apache.http.auth.AuthScope;
import org.apache.http.auth.UsernamePasswordCredentials;
import org.apache.http.client.CredentialsProvider;
import org.apache.http.impl.client.BasicCredentialsProvider;
import org.apache.http.impl.nio.client.HttpAsyncClientBuilder;

import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.action.delete.DeleteResponse;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestClientBuilder;
import org.elasticsearch.client.RestHighLevelClient;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

public class RestClientTest63 {
    public static void main(String[] args) {

        // Use basic access authentication for the Elasticsearch cluster.
        final CredentialsProvider credentialsProvider = new BasicCredentialsProvider();
       // Use the username and password that are specified when you create the Elasticsearch cluster. You can also use the username and password to log on to the Kibana console.
        credentialsProvider.setCredentials(AuthScope.ANY, new UsernamePasswordCredentials("{Username}", "{Password}"));

       // Create a Java REST client by using the builder and configure HttpClientConfigCallback for the HTTP client.
       // Specify the public endpoint of the Elasticsearch cluster. You can obtain the endpoint from the Basic Information page of the cluster.
        RestClientBuilder builder = RestClient.builder(new HttpHost("{Elasticsearch cluster endpoint}", 9200, "http"))
                .setHttpClientConfigCallback(new RestClientBuilder.HttpClientConfigCallback() {
                    @Override
                    public HttpAsyncClientBuilder customizeHttpClient(HttpAsyncClientBuilder httpClientBuilder) {
                        return httpClientBuilder.setDefaultCredentialsProvider(credentialsProvider);
                    }
                });

        // Create a RestHighLevelClient instance by using the REST low-level client builder.
        RestHighLevelClient highClient = new RestHighLevelClient(builder);

        try {
            // Create a request.
            Map<String, Object> jsonMap = new HashMap<>();
           // field_01 and field_02 are field names, and value_01 and value_02 are the values of field_01 and field_02.
            jsonMap.put("{field_01}", "{value_01}");
            jsonMap.put("{field_02}", "{value_02}");
            // index_name is the index name, type_name is the type name, and doc_id is the document ID.       
            IndexRequest indexRequest = new IndexRequest("{index_name}", "{type_name}", "{doc_id}").source(jsonMap);

            // Run the following command:
            IndexResponse indexResponse = highClient.index(indexRequest);

            long version = indexResponse.getVersion();

            System.out.println("Index document successfully! " + version);
            // index_name is the index name, type_name is the type name, and doc_id is the document ID. The index name, type name, and document ID are the same as those you specified when you create the index.
            DeleteRequest request = new DeleteRequest("{index_name}", "{type_name}", "{doc_id}");
            DeleteResponse deleteResponse = highClient.delete(request);

            System.out.println("Delete document successfully!") ;

            highClient.close();

        } catch (IOException ioException) {
            // Handle exceptions.
        }
    }
}
```

You can replace the parameters in the preceding sample code that are contained in braces `{}` with service-specific parameters. For more information, see the code comments.

For more information about the features of Java High Level REST Client, see [Java High Level REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.3/java-rest-high.html).

