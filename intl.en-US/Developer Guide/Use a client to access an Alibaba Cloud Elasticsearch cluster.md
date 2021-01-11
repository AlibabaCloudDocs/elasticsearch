# Use a client to access an Alibaba Cloud Elasticsearch cluster

This topic describes how to use a PHP, Python, or Java client to access an Alibaba Cloud Elasticsearch cluster. It also provides sample code and usage notes of each type of client.

## Preparations

Before you use a client to access an Alibaba Cloud Elasticsearch cluster, make sure that you have completed the following operations:

-   Install a JDK. The JDK version must be 1.8 or later.

    For more information, see [Install the JDK](/intl.en-US/Best Practices/Migrate and synchronize MySQL data/RDS synchronization/Use Canal to synchronize MySQL data to Alibaba Cloud Elasticsearch.md).

-   Create an Alibaba Cloud Elasticsearch cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

-   Enable the Auto Indexing feature for the Elasticsearch cluster.

    For more information, see [Enable the Auto Indexing feature](/intl.en-US/Elasticsearch Instances Management/Quick Start/Step 2: (Optional) Configure a cluster.md).

-   Configure a whitelist for the Elasticsearch cluster to ensure normal communication among networks.
    -   If the server that runs code is located in an Internet environment, you can access the Elasticsearch cluster by using its public endpoint. Before you access the cluster, you must enable the Public Network Access feature for the cluster and add the public IP address of the server to the Internet whitelist of the cluster. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md).

        **Note:**

        -   If you are using a public network, add the IP address of the jump server that controls outbound traffic of the public network to the whitelist.
        -   You can also add 0.0.0.0/0 to the whitelist to allow requests from all IPv4 addresses. If you make this configuration, all public IP addresses can be used to access the Elasticsearch cluster. This poses security risks. We recommend that you evaluate the risks before you make this configuration.
    -   If the server that runs code is located in the same Virtual Private Cloud \(VPC\) as the Elasticsearch cluster, you can access the cluster by using its internal endpoint. Before you access the cluster, make sure that the internal IP address of the server is added to the VPC whitelist of the cluster. By default, 0.0.0.0/0 is added to the whitelist.

## PHP client

**Warning:**

-   The default connection pool provided by the PHP client of Elasticsearch is not suitable for accessing services on the cloud. Alibaba Cloud Elasticsearch handles requests sent from clients based on load balancing. Therefore, a PHP client must use SimpleConnectionPool as the connection pool. Otherwise, when your Alibaba Cloud Elasticsearch cluster is restarted, a connection error occurs.
-   A PHP client must be configured to reconnect to an Elasticsearch cluster after the client is disconnected. If the PHP client cannot reconnect to the cluster, an error may occur \(for example, the "No enabled connection" error message is reported\) during a restart of the cluster even if SimpleConnectionPool is used as the connection pool.

To test the connectivity to your Elasticsearch cluster, connect a PHP client to the cluster over port 9200. For more information, see [elasticsearch-php](https://www.elastic.co/guide/cn/elasticsearch/php/current/_installation_2.html). Sample code:

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

|Parameter|Description|
|---------|-----------|
|host|The internal or public endpoint of your Elasticsearch cluster. You can obtain the endpoint from the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page of the cluster.|
|user|The username of your Elasticsearch cluster. The default username is elastic.|
|pass|The password of your Elasticsearch cluster. The password of the elastic account is specified when you create the cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|

## Python client

To test the connectivity to your Elasticsearch cluster, connect a Python client to the cluster over port 9200. For more information, see [elasticsearch-py](https://www.elastic.co/guide/en/elasticsearch/client/python-api/current/index.html). Sample code:

```
from elasticsearch import Elasticsearch, RequestsHttpConnection
import certifi
es = Elasticsearch(
    ['<HOST>'],
    http_auth=('username', 'password'),
    port=9200,
    use_ssl=False
)
res = es.index(index="my_index", doc_type="my_type", id=1, body={"title": "One", "tags": ["ruby"]})
res = es.get(index="my_index", doc_type="my_type", id=1)
print(res['_source'])
```

|Parameter|Description|
|---------|-----------|
|<HOST\>|The internal or public endpoint of your Elasticsearch cluster. You can obtain the endpoint from the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page of the cluster.|
|username|The username of your Elasticsearch cluster. The default username is elastic.|
|password|The password of your Elasticsearch cluster. The password of the elastic account is specified when you create the cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|

## Java client

Java clients include Transport Client, Low Level REST Client, and High Level REST Client. For more information about the sample code for each type of client, see [Java API](/intl.en-US/Developer Guide/Java API/Overview.md).

**Note:** Java Transport Client communicates with Elasticsearch over TCP. If the client communicates with Elasticsearch of a version that does not match its version, incompatibility issues may occur. In later versions of open source Elasticsearch, Transport Client is deprecated. If you use Transport Client 5.5 to access an Elasticsearch V5.5 cluster or use Transport Client 5.6 to access an Elasticsearch V5.6 cluster, the system displays the "NoNodeAvailableException" error message. To ensure version compatibility, we recommend that you use Transport Client 5.3.3 or [Java Low Level REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/5.5/_basic_authentication.html) to access an Elasticsearch cluster. If you use Transport Client, set `client.transport.sniff` to false.

## Go client

To test the connectivity to your Elasticsearch cluster, connect a Go client to the cluster over port 9200.

Before you use a Go client to access an Elasticsearch cluster, you must install a compilation environment for Go. For more information, see [open source Elasticsearch documentation](https://github.com/golang/go). In the following sample code, Go 1.13.4 is used.

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
    if err ! = nil {
        log.Println(err)
        return
    }

    exists, err := client.IndexExists("index_test").Do(context.Background())   //<5>
    if err ! = nil {
        log.Println(err)
    }
    log.Println(exists)
}
```

|Parameter|Description|
|---------|-----------|
|<1\>: url|The endpoint of your Elasticsearch cluster. You can obtain the endpoint from the [Basic Information](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md) page of the cluster.|
|<2\>: username|The username of your Elasticsearch cluster. The default username is elastic.|
|<3\>: password|The password of your Elasticsearch cluster. The password of the elastic account is specified when you create the cluster. If you forget the password, you can reset it. For more information about the procedure and precautions for resetting the password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|
|<4\>: sniff|Set the value to false to prevent sniffing on connections. If you set the value to true, connection failures occur.|
|<5\>: exists, err|This parameter creates an index named index\_test on the Elasticsearch cluster and returns log data that contains the connection information. Replace the index name with the actual name of your index.|

If the client is connected to the Elasticsearch cluster, the following result is returned:

```
2019/11/29 10:07:37 true
```

For more information about the clients in other languages, see [HTTP/REST Clients and Security](https://www.elastic.co/guide/en/x-pack/current/http-clients.html).

