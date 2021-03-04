# High Level REST Client（7.x）

本文基于Java High Level REST Client 7.x版本，为您介绍Elasticsearch Java API的用法。

## 准备工作

-   创建阿里云Elasticsearch实例，版本要求大于等于Java High Level REST Client的版本。

    本文创建一个7.4.0版本的实例，创建方法请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/Elasticsearch/管理实例/创建阿里云Elasticsearch实例.md)。

    **说明：** High Level Client能够向上兼容，例如7.4.0版本的Java High Level REST Client能确保与大于等于7.4.0版本的Elasticsearch集群通信。为了保证最大程度地使用最新版客户端的特性，推荐High Level Client版本与集群版本一致。

-   开启阿里云Elasticsearch实例的自动创建索引功能。

    具体操作步骤请参见[开启自动创建索引](/intl.zh-CN/Elasticsearch/快速访问与配置.md)。

    如果未开启会提示如下报错。

    ![报错](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8769559951/p97345.png)

-   配置阿里云Elasticsearch实例的白名单，确保网络互通。
    -   如果运行Java代码的服务器在公网环境下，可通过阿里云Elasticsearch实例的公网地址进行连通。连通前，需要开启阿里云Elasticsearch实例的公网地址，并修改公网地址访问白名单，将服务器的公网IP地址加入白名单中。具体操作步骤请参见[配置Elasticsearch公网或私网访问白名单](/intl.zh-CN/Elasticsearch/安全配置/配置ES公网或私网访问白名单.md)。

        **说明：**

        -   如果您使用的是WIFI、宽带等网络，需要将公网出口的跳板机IP地址配置进去。
        -   您也可以将白名单配置为0.0.0.0/0，允许所有IPv4地址访问阿里云Elasticsearch实例。此配置会导致实例完全暴露在公网中，增加安全风险，配置前请确认您是否可以接受这个风险。
    -   如果运行Java代码的服务器与阿里云Elasticsearch实例在同一专有网络VPC（Virtual Private Cloud）中，可通过阿里云Elasticsearch实例的内网地址进行连通。连通前，需要确保VPC私网访问白名单（默认为0.0.0.0/0）中已添加了服务器的内网IP地址。
-   安装Java，要求JDK版本为1.8及以上。
-   创建Java Maven工程，并将如下的[pom依赖](#section_zns_56a_i8r)添加到Java工程的pom.xml文件中。

## pom依赖

```
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>7.4.0</version>
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

单击下载[完整示例代码](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/33813/cn_zh/1593655159726/es7.4-demo.zip)。

以下代码使用Index API创建索引，使用Delete API删除该索引，并演示了在JVM内存分配比较有限的客户端环境中，通过调整ResponseConsumer配置，限制异步响应所占用的缓存的大小。

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
import org.elasticsearch.client.*;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

public class RestClientTest74 {

    private static final RequestOptions COMMON_OPTIONS;

    static {
        RequestOptions.Builder builder = RequestOptions.DEFAULT.toBuilder();

        // 默认缓存限制为100MB，此处修改为30MB。
        builder.setHttpAsyncResponseConsumerFactory(
                new HttpAsyncResponseConsumerFactory
                        .HeapBufferedResponseConsumerFactory(30 * 1024 * 1024));
        COMMON_OPTIONS = builder.build();
    }

    public static void main(String[] args) {
        // 阿里云Elasticsearch集群需要basic auth验证。
        final CredentialsProvider credentialsProvider = new BasicCredentialsProvider();
        //访问用户名和密码为您创建阿里云Elasticsearch实例时设置的用户名和密码，也是Kibana控制台的登录用户名和密码。
        credentialsProvider.setCredentials(AuthScope.ANY, new UsernamePasswordCredentials("{访问用户名}", "{访问密码}"));

        // 通过builder创建rest client，配置http client的HttpClientConfigCallback。
        // 单击所创建的Elasticsearch实例ID，在基本信息页面获取公网地址，即为ES集群地址。
        RestClientBuilder builder = RestClient.builder(new HttpHost("{ES集群地址}", 9200))
                .setHttpClientConfigCallback(new RestClientBuilder.HttpClientConfigCallback() {
                    @Override
                    public HttpAsyncClientBuilder customizeHttpClient(HttpAsyncClientBuilder httpClientBuilder) {
                        return httpClientBuilder.setDefaultCredentialsProvider(credentialsProvider);
                    }
                });

        // RestHighLevelClient实例通过REST low-level client builder进行构造。
        RestHighLevelClient highClient = new RestHighLevelClient(builder);

        try {
            // 创建request。
            Map<String, Object> jsonMap = new HashMap<>();
            // field_01、field_02为字段名，value_01、value_02为对应的值。
            jsonMap.put("{field_01}", "{value_01}");
            jsonMap.put("{field_02}", "{value_02}");
            //index_name为索引名称；type_name为类型名称,7.0及以上版本必须为_doc；doc_id为文档的id。
            IndexRequest indexRequest = new IndexRequest("{index_name}", "_doc", "{doc_id}").source(jsonMap);

            // 同步执行，并使用自定义RequestOptions（COMMON_OPTIONS）。
            IndexResponse indexResponse = highClient.index(indexRequest, COMMON_OPTIONS);

            long version = indexResponse.getVersion();

            System.out.println("Index document successfully! " + version);
            //index_name为索引名称；type_name为类型名称,7.0及以上版本必须为_doc；doc_id为文档的id。与以上创建索引的名称和id相同。
            DeleteRequest request = new DeleteRequest("{index_name}", "_doc", "{doc_id}");
            DeleteResponse deleteResponse = highClient.delete(request, COMMON_OPTIONS);

            System.out.println("Delete document successfully! \n" + deleteResponse.toString());

            highClient.close();

        } catch (IOException ioException) {
            // 异常处理。
        }
    }
}
```

以上示例代码中带`{}`的参数需要替换为您具体业务的参数，详情请参见代码注释。

更多Java High Level REST Client的使用特性，请参见[Java High Level REST Client官方文档](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.7/java-rest-high.html)。

