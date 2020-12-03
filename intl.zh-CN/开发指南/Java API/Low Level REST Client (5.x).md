# Low Level REST Client \(5.x\)

本文基于Java Low Level REST Client 5.x版本，为您介绍Elasticsearch Java API的用法。

## 注意事项

-   本文提供的Low Level REST Client示例主要适用于阿里云Elasticsearch 5.5.3版本，不适用于6.3.2版本。如果您的Elasticsearch实例是6.3.2版本，可参见Elasticsearch [Java REST Client 6.3.2](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.3/index.html)官方文档进行配置。
-   Java REST Client版本需要与Elasticsearch实例版本保持一致。

## 准备工作

-   安装Java，要求JDK版本为1.8及以上。

    安装方法请参见[安装JDK](/intl.zh-CN/最佳实践/数据库同步/RDS MySQL同步/通过Canal将MySQL数据同步到阿里云Elasticsearch.md)。

-   创建阿里云Elasticsearch实例，版本为5.5.3。

    创建方法请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

-   开启阿里云Elasticsearch实例的自动创建索引功能。

    具体操作步骤请参见[开启自动创建索引](/intl.zh-CN/快速入门/步骤二：配置实例（可选）.md)。

    如果未开启会提示如下报错。

    ![报错](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8769559951/p97345.png)

-   配置阿里云Elasticsearch实例的白名单，确保网络互通。
    -   如果运行Java代码的服务器在公网环境下，可通过阿里云Elasticsearch实例的公网地址进行连通。连通前，需要开启阿里云Elasticsearch实例的公网地址，并修改公网地址访问白名单，将服务器的公网IP地址加入白名单中。具体操作步骤请参见[配置ES公网或私网访问白名单](/intl.zh-CN/实例管理/安全配置/配置ES公网或私网访问白名单.md)。

        **说明：**

        -   如果您使用的是WIFI、宽带等网络，需要将公网出口的跳板机IP地址配置进去。
        -   您也可以将白名单配置为0.0.0.0/0，允许所有IPv4地址访问阿里云Elasticsearch实例。此配置会导致实例完全暴露在公网中，增加安全风险，配置前请确认您是否可以接受这个风险。
    -   如果运行Java代码的服务器与阿里云Elasticsearch实例在同一专有网络VPC（Virtual Private Cloud）中，可通过阿里云Elasticsearch实例的内网地址进行连通。连通前，需要确保VPC私网访问白名单（默认为0.0.0.0/0）中已添加了服务器的内网IP地址。
-   创建Java Maven工程，并将下文的[pom依赖](#section_pi7_1uv_yit)添加到Java工程的pom.xml文件中。

## pom依赖

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

## 示例

单击下载[完整示例代码](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/33813/cn_zh/1593402410101/es5.5-demo.zip)。

通过Java REST Client访问阿里云Elasticsearch的9200端口进行测试，示例代码如下。

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
        // 单击所创建的Elasticsearch实例ID，在基本信息页面获取公网地址，即为HOST。
        RestClient restClient = RestClient.builder(new HttpHost("HOST", 9200))
                .setHttpClientConfigCallback(new RestClientBuilder.HttpClientConfigCallback() {
                    @Override
                    public HttpAsyncClientBuilder customizeHttpClient(HttpAsyncClientBuilder httpClientBuilder) {
                        return httpClientBuilder.setDefaultCredentialsProvider(credentialsProvider);
                    }
                }).build();
        try {
            //field_01、field_02为字段名，value_01、value_02为对应的值。
            HttpEntity entity = new NStringEntity("{\n\"field_01\" : \"value_01\"\n,\n\"field_02\" : \"value_02\"\n}", ContentType.APPLICATION_JSON);
            //index_name为索引名称；type_name为类型名称；doc_id为文档的id。
            Response indexResponse = restClient.performRequest(
                    "PUT",
                    "/index_name/type_name/doc_id",
                    Collections.<String, String>emptyMap(),
                    entity);
            //index_name为索引名称；type_name为类型名称；doc_id为文档的id。与以上创建索引的名称和id相同。
            Response response = restClient.performRequest("GET", "/index_name/type_name/doc_id",
                    Collections.singletonMap("pretty", "true"));
            System.out.println(EntityUtils.toString(response.getEntity()));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

|参数|说明|
|--|--|
|`USER NAME`|替换为访问阿里云Elasticsearch实例的用户名。|
|`PASSWORD`|替换为访问阿里云Elasticsearch实例的密码。|
|`HOST`|替换为阿里云Elasticsearch实例的内网或外网地址。可在实例的基本信息页面获取，获取方法请参见[查看实例的基本信息](/intl.zh-CN/实例管理/管理实例/查看实例的基本信息.md)。|

