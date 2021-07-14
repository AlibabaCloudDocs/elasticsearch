# 通过客户端访问阿里云Elasticsearch

本文介绍使用PHP、Python、Java和Go语言访问阿里云Elasticsearch的方法，并提供了示例代码和注意事项供您参考。

## 准备工作

在使用客户端访问阿里云Elasticsearch实例前，您需要先完成以下准备工作：

-   安装Java，要求JDK版本为1.8及以上。

    安装方法请参见[安装JDK](/intl.zh-CN/最佳实践/数据库同步/RDS MySQL同步/通过Canal将MySQL数据同步到阿里云Elasticsearch.md)。

-   创建阿里云Elasticsearch实例。

    具体操作步骤请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/Elasticsearch/管理实例/创建阿里云Elasticsearch实例.md)。

-   开启阿里云Elasticsearch实例的自动创建索引功能。

    具体操作步骤请参见[配置YML参数](/intl.zh-CN/Elasticsearch/ES集群配置/配置YML参数.md)。

-   配置阿里云Elasticsearch实例的白名单，确保网络互通。
    -   如果运行代码的服务器在公网环境下，可通过阿里云Elasticsearch实例的公网地址进行连通。连通前，需要开启阿里云Elasticsearch实例的公网地址，并修改公网地址访问白名单，将服务器的公网IP地址加入白名单中。具体操作步骤请参见[配置Elasticsearch公网或私网访问白名单](/intl.zh-CN/Elasticsearch/安全配置/配置ES公网或私网访问白名单.md)。

        **说明：**

        -   如果您使用的是WIFI、宽带等网络，需要将公网出口的跳板机IP地址配置进去。
        -   您也可以将白名单配置为0.0.0.0/0，允许所有IPv4地址访问阿里云Elasticsearch实例。此配置会导致实例完全暴露在公网中，增加安全风险，配置前请确认您是否可以接受这个风险。
    -   如果运行代码的服务器与阿里云Elasticsearch实例在同一专有网络VPC（Virtual Private Cloud）中，可通过阿里云Elasticsearch实例的内网地址进行连通。连通前，需要确保VPC私网访问白名单（默认为0.0.0.0/0）中已添加了服务器的内网IP地址。

## PHP语言

**警告：**

-   Elasticsearch的PHP客户端提供的默认连接池并不适合云上环境。阿里云Elasticsearch提供了负载均衡的域名服务，因此PHP客户端访问程序必须使用SimpleConnectionPool作为连接池，否则在触发阿里云Elasticsearch重启操作时会出现访问连接异常的问题。
-   PHP客户端访问程序必须具备访问连接失败重连的机制。因为在PHP客户端访问程序已使用SimpleConnectionPool作为连接池后，不排除在触发阿里云Elasticsearch重启操作时还会出现访问连接异常问题（例如提示No enabled connection），因此PHP客户端需要具备访问连接失败重连的机制。

通过PHP客户端访问阿里云Elasticsearch的9200端口进行测试，示例代码如下，详情请参见[elasticsearch-php](https://www.elastic.co/guide/cn/elasticsearch/php/current/_installation_2.html)。

```
<?php
require 'vendor/autoload.php';
use Elasticsearch\ClientBuilder;

$client = ClientBuilder::create()->setHosts([
  [
    'host'   => '<HOST>',
    'port'   => '9200',
    'scheme' => 'http',
    'user'   => '<USER NAME>',
    'pass'   => '<PASSWORD>'
  ]
])->setConnectionPool('\Elasticsearch\ConnectionPool\SimpleConnectionPool', [])
  ->setRetries(10)->build();

$indexParams = [
  'index'  => 'my_index',
  'type'   => 'my_type',
  'id'     => '1',
  'body'   => ['testField' => 'abc'],
  'client' => [
    'timeout'         => 10,
    'connect_timeout' => 10
  ]
];
$indexResponse = $client->index($indexParams);
print_r($indexResponse);

$searchParams = [
  'index'  => 'my_index',
  'type'   => 'my_type',
  'body'   => [
    'query' => [
      'match' => [
        'testField' => 'abc'
      ]
    ]
  ],
  'client' => [
    'timeout'         => 10,
    'connect_timeout' => 10
  ]
];
$searchResponse = $client->search($searchParams);
print_r($searchResponse);
?>
```

|参数|说明|
|--|--|
|host|阿里云Elasticsearch实例[基本信息](/intl.zh-CN/Elasticsearch/管理实例/查看实例的基本信息.md)页面中的内网或外网地址。|
|user|访问阿里云Elasticsearch实例的用户名，默认为elastic。|
|pass|访问阿里云Elasticsearch实例的密码。elastic用户的密码在创建实例时设定，如果忘记可进行重置，重置密码的注意事项和操作步骤请参见[重置实例访问密码](/intl.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|

## Python语言

通过Python客户端访问阿里云Elasticsearch的9200端口进行测试，示例代码如下，详情请参见[elasticsearch-py](https://www.elastic.co/guide/en/elasticsearch/client/python-api/current/index.html)。

```
from elasticsearch import Elasticsearch, RequestsHttpConnection
import certifi
es = Elasticsearch(
    ['<HOST>'],
    http_auth=('<USERNAME>', '<PASSWORD>'),
    port=9200,
    use_ssl=False
)
res = es.index(index="my_index", doc_type="my_type", id=1, body={"title": "One", "tags": ["ruby"]})
res = es.get(index="my_index", doc_type="my_type", id=1)
print(res['_source'])
```

|参数|说明|
|--|--|
|<HOST\>|阿里云Elasticsearch实例[基本信息](/intl.zh-CN/Elasticsearch/管理实例/查看实例的基本信息.md)页面中的内网或外网地址。|
|<USERNAME\>|访问阿里云Elasticsearch实例的用户名，默认为elastic。|
|<PASSWORD\>|访问阿里云Elasticsearch实例的密码。elastic用户的密码在创建实例时设定，如果忘记可进行重置，重置密码的注意事项和操作步骤请参见[重置实例访问密码](/intl.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|

## Java语言

Java客户端包括3种类型：Transport Client、Low Level REST Client和High Level REST Client，各类型的代码示例请参见[Java API](/intl.zh-CN/开发指南/Java API/概述.md)章节。

**说明：** 由于Java Transport Client通过TCP与Elasticsearch进行通信，因此当客户端与不同版本的Elasticsearch通信时，会存在兼容性问题，所以官方在高版本集群中已弃用Transport Client。如果您已经创建了5.5或5.6版本的Elasticsearch集群，分别使用Transport Client 5.5或5.6版本与Elasticsearch建立连接时会提示NoNodeAvailableException的错误。推荐使用Transport Client 5.3.3或[Java Low Level REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/5.5/_basic_authentication.html)来访问Elasticsearch集群，以保障版本的兼容性。如果使用Transport Client，需要在代码中将`client.transport.sniff`设置为false。

## Go语言

通过Go语言访问阿里云Elasticsearch的9200端口进行测试，示例代码如下。

使用Go语言连接阿里云Elasticsearch前，您需要先安装Go编译环境，详情请参见[官方文档](https://github.com/golang/go)。以下示例代码使用Go 1.13.4版本。

```
package main

import (
    "context"
    "gopkg.in/olivere/elastic.v5"
    "gopkg.in/olivere/elastic.v5/config"
    "log"
)

const (
    url      = "http://es-cn-xxxxxx.public.elasticsearch.aliyuncs.com:9200"     //<1>
    username = "elastic"        //<2>
    password = "password"       //<3>
)

func main() {
    var sniff = false    //<4>  
    cfg := &config.Config{
        URL:      url,
        Username: username,
        Password: password,
    }

    cfg.Sniff = &sniff          
    var client, err = elastic.NewClientFromConfig(cfg)
    if err != nil {
        log.Println(err)
        return
    }

    exists, err := client.IndexExists("index_test").Do(context.Background())   //<5>
    if err != nil {
        log.Println(err)
    }
    log.Println(exists)
}
```

|参数|说明|
|--|--|
|<1\>：url|阿里云Elasticsearch的访问域名，可在实例的[基本信息](/intl.zh-CN/Elasticsearch/管理实例/查看实例的基本信息.md)页面获取。|
|<2\>：username|阿里云Elasticsearch的访问账号，默认为elastic。|
|<3\>：password|阿里云Elasticsearch的访问密码。elastic用户的密码在创建实例时设定，如果忘记可进行重置，重置密码的注意事项和操作步骤请参见[重置实例访问密码](/intl.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|
|<4\>：sniff|必须设置为false，防止二次探测，导致连接失败。|
|<5\>：exists, err|在阿里云Elasticsearch上新建一个索引index\_test（需要替换为您的索引名称），并根据连接状态返回日志信息。|

连接成功后，返回如下结果。

```
2019/11/29 10:07:37 true
```

更多语言示例请参见[HTTP/REST Clients and Security](https://www.elastic.co/guide/en/x-pack/current/http-clients.html)。

