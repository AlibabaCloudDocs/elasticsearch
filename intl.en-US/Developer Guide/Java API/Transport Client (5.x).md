---
keyword: use Transport Client to access an Alibaba Cloud Elasticsearch cluster
---

# Transport Client \(5.x\)

This topic describes how to use Transport Client 5.3.3 to access an Alibaba Cloud Elasticsearch V5.X cluster.

## Precautions

-   We recommend that you use Transport Client 5.3.3 to access Elasticsearch clusters. If you use Transport Client 5.5 or 5.6 to access an Elasticsearch cluster, the error message "NoNodeAvailableException" is displayed.
-   Transport Client communicates with Elasticsearch over TCP. If it communicates with Elasticsearch of a version that does not match its version, incompatibility issues may occur. In later versions of open source Elasticsearch, Transport Client is deprecated. We recommended that you use [Java Low Level REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/5.5/_basic_authentication.html) to access Elasticsearch clusters to ensure version compatibility.

## Preparations

-   Install a JDK. The JDK version must be 1.8 or later.

    For more information, see [Install the JDK](/intl.en-US/Best Practices/Migrate and synchronize MySQL data/RDS synchronization/Use Canal to synchronize MySQL data to Alibaba Cloud Elasticsearch.md).

-   Create an Alibaba Cloud Elasticsearch V5.5.3 cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/Create an Alibaba Cloud Elasticsearch cluster.md).

-   Enable the Auto Indexing feature for the Elasticsearch cluster.

    For more information, see [t1605396.md\#section\_pcn\_1xy\_1l2](/intl.en-US/Elasticsearch Instances Management/Step 2: (Optional) Configure a cluster.md).

    If the Auto Indexing feature is not enabled, the following error is reported.

    ![Error](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5487649951/p97345.png)

-   Configure a whitelist for the Elasticsearch cluster to ensure normal communication among networks.
    -   If the server that runs Java code is located in an Internet environment, you can access the Elasticsearch cluster by using its public endpoint. Before you access the cluster, you must enable the Public Network Access feature for the cluster and add the public IP address of the server to the Internet whitelist of the cluster. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).

        **Note:**

        -   If you are using a public network, add the IP address of the jump server that controls outbound traffic of the public network to the whitelist.
        -   You can also add 0.0.0.0/0 to the whitelist to allow requests from all IPv4 addresses. If you make this configuration, all public IP addresses can be used to access the Elasticsearch cluster. This poses security risks. We recommend that you evaluate the risks before you make this configuration.
    -   If the server that runs Java code is located in the same Virtual Private Cloud \(VPC\) as the Elasticsearch cluster, you can access the cluster by using its internal endpoint. Before you access the cluster, make sure that the internal IP address of the server is added to the VPC whitelist of the cluster. By default, 0.0.0.0/0 is added to the whitelist.
-   Create a Java Maven project and add the following [Project Object Model \(POM\) dependencies](#section_53p_is5_80a) to the pom.xml file of the Java project.

## POM dependencies

```
<repositories>
    <! -- add the elasticsearch repo -->
    <repository>
        <id>elasticsearch-releases</id>
        <url>https://artifacts.elastic.co/maven</url>
        <releases>
            <enabled>true</enabled>
        </releases>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
<dependencies>
    <dependency>
        <groupId>org.elasticsearch.client</groupId>
        <artifactId>x-pack-transport</artifactId>
        <version>5.3.3</version>
     </dependency>
     <dependency>
        <groupId>org.elasticsearch</groupId>
        <artifactId>elasticsearch</artifactId>
        <version>5.3.3</version>
     </dependency>
     <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-api</artifactId>
        <version>2.7</version>
     </dependency>
     <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.7</version>
     </dependency>
</dependencies>
```

## Example

You can download the [complete sample code](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/33813/cn_zh/1596770729238/es-transport5.3-demo.zip).

```
import org.elasticsearch.action.get.GetResponse;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.client.transport.TransportClient;
import org.elasticsearch.common.settings.Settings;
import org.elasticsearch.common.transport.InetSocketTransportAddress;
import org.elasticsearch.xpack.client.PreBuiltXPackTransportClient;

import static org.elasticsearch.common.xcontent.XContentFactory.*;

import java.net.InetAddress;

public class TransportClientDemo {

    public static void main(String[] args) {
        try {
            TransportClient client = new PreBuiltXPackTransportClient(Settings.builder()
                    .put("cluster.name", "es-cn-n6w1rux8i000w****") //Set the value to the ID of the Elasticsearch cluster.
                    .put("xpack.security.user", "elastic:es_password") //Set the value to the username and password of the Elasticsearch cluster.
                    . put("client.transport.sniff", false) //set the value to false.
                    .build())
                    .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("es-cn-n6w1rux8i000w****.public.elasticsearch.aliyuncs.com"), 9300));//Specify the endpoint and port number of the Elasticsearch cluster.

            //You can modify the following code as required. For example, you can change the index name, index type, and document ID.
            IndexResponse idxResp3 = client.prepareIndex("test_index", "test_type", "333")
                    .setSource(jsonBuilder()
                            .startObject()
                            .field("user_id", "333")
                            .field("email", "test@aliyun.com")
                            .endObject()
                    )
                    .get();

            System.out.println(idxResp3.toString());


            GetResponse getResp =
                    client.prepareGet().setIndex("test_index").setType("test_type").setId("333").execute().get();
            System.out.println(getResp.getSourceAsString());
            client.close();

        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

|Parameter|Description|
|---------|-----------|
|`cluster.name`|Set the value to the ID of the Elasticsearch cluster. You can obtain the ID from the Basic Information page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|
|`client.transport.sniff`|Set the value to false.|
|`xpack.security.user`|Set the value to the username and password of the Elasticsearch cluster.|
|`InetAddress.getByName()`|Specify the endpoint and port number of the Elasticsearch cluster that you want to access. **Note:** The endpoint cannot start with http. |

