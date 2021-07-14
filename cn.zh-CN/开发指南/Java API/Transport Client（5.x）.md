---
keyword: Transport Client访问es
---

# Transport Client（5.x）

您可以通过Transport Client 5.3.3访问阿里云Elasticsearch 5.x版本的实例。本文介绍相关配置说明。

## 注意事项

-   建议您使用5.3.3版本的Transport Client来访问Elasticsearch集群。使用5.5或5.6版本访问Elasticsearch集群会提示NoNodeAvailableException的错误。
-   由于Transport Client通过TCP与Elasticsearch进行通信，因此当客户端与不同版本的Elasticsearch通信时，会存在兼容性问题，所以官方在高版本集群中已弃用Transport Client。建议优先使用[Java Low Level REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/5.5/_basic_authentication.html)来访问Elasticsearch集群，以保障版本的兼容性。

## 准备工作

-   安装Java，要求JDK版本为1.8及以上。

    安装方法请参见[安装JDK](/cn.zh-CN/最佳实践/数据库同步/RDS MySQL同步/通过Canal将MySQL数据同步到阿里云Elasticsearch.md)。

-   创建阿里云Elasticsearch实例，版本为5.5.3。

    创建方法请参见[t134282.md\#](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)。

-   开启阿里云Elasticsearch实例的自动创建索引功能。

    具体操作步骤请参见[配置YML参数](/cn.zh-CN/Elasticsearch/集群配置/配置YML参数.md)。

    如果未开启会提示如下报错。

    ![报错](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8769559951/p97345.png)

-   配置阿里云Elasticsearch实例的白名单，确保网络互通。
    -   如果运行Java代码的服务器在公网环境下，可通过阿里云Elasticsearch实例的公网地址进行连通。连通前，需要开启阿里云Elasticsearch实例的公网地址，并修改公网地址访问白名单，将服务器的公网IP地址加入白名单中。具体操作步骤请参见[配置Elasticsearch公网或私网访问白名单](/cn.zh-CN/Elasticsearch/安全配置/配置Elasticsearch公网或私网访问白名单.md)。

        **说明：**

        -   如果您的客户端处在家庭网络或公司局域网中，您需要将局域网的公网出口IP地址添加到白名单中，而非客户端机器的内网机制。建议您通过[淘宝IP地址库](http://myip.ipip.net/)查看您当前使用的公网IP。
        -   您也可以将白名单配置为0.0.0.0/0，允许所有IPv4地址访问阿里云Elasticsearch实例。此配置会导致实例完全暴露在公网中，增加安全风险，配置前请确认您是否可以接受这个风险。
    -   如果运行Java代码的服务器与阿里云Elasticsearch实例在同一专有网络VPC（Virtual Private Cloud）中，可通过阿里云Elasticsearch实例的内网地址进行连通。连通前，需要确保VPC私网访问白名单（默认为0.0.0.0/0）中已添加了服务器的内网IP地址。
-   创建Java Maven工程，并将下文的[pom依赖](#section_53p_is5_80a)添加到Java工程的pom.xml文件中。

## pom依赖

```
<repositories>
    <!-- add the elasticsearch repo -->
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

## 示例

单击下载[完整示例代码](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/33813/cn_zh/1596770729238/es-transport5.3-demo.zip)。

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
                    .put("cluster.name", "es-cn-n6w1rux8i000w****") //替换为对应阿里云ES实例的ID。
                    .put("xpack.security.user", "elastic:es_password") //阿里云ES实例的用户名、密码。
                    .put("client.transport.sniff", false) //设置sniff为false。
                    .build())
                    .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("es-cn-n6w1rux8i000w****.public.elasticsearch.aliyuncs.com"), 9300));//指定域名及端口，替换为对应阿里云ES实例的域名。

            //下面这段代码可以根据实际情况进行修改，指定index，type及文档id。
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

|参数|描述|
|--|--|
|`cluster.name`|阿里云Elasticsearch实例的ID，可在实例的基本信息页面获取，获取方法请参见[查看实例的基本信息](/cn.zh-CN/Elasticsearch/实例管理/查看实例的基本信息.md)。|
|`client.transport.sniff`|嗅探，请设置为false。|
|`xpack.security.user`|阿里云Elasticsearch实例的用户名和密码。|
|`InetAddress.getByName()`|请在此方法的参数中指定访问阿里云Elasticsearch实例的域名及端口号。 **说明：** 域名不能加http。 |

