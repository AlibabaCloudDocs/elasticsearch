# Overview

This topic describes the principles, version compatibility, and usage examples of Java clients for Elasticsearch. You can use a Java client to interact with Elasticsearch and perform retrieval and analytics tasks.

## Migrate existing code from Transport Client to a REST client

Transport Client is a special client that was developed with the first version of Elasticsearch. It communicates with Elasticsearch over TCP. If it communicates with Elasticsearch of a version that does not match its version, incompatibility issues may occur. For more information, see [Motivations around a new Java client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.7/_motivations_around_a_new_java_client.html).

Elasticsearch released Java Low Level REST Client in 2016. Java Low Level REST Client is developed based on the Apache HTTP client and can communication with all versions of Elasticsearch clusters over HTTP. Elasticsearch also released High Level REST Client based on Low Level REST Client.

Transport Client falls into disuse in Elasticsearch 7.0 and is no longer available in Elasticsearch 8.0. We recommend that you use Java REST clients. A Java REST client uses HTTP requests to handle serialization issues for requests and responses. This makes your business development easier.

**Note:**

-   You can use Transport Client to access only Alibaba Cloud Elasticsearch V5.5 or V5.6 clusters over port 9300. Alibaba Cloud Elasticsearch clusters of V6.X or later does not support Transport Client.
-   If you use Transport Client 5.5 or 5.6 to access an Alibaba Cloud Elasticsearch cluster, the system displays the "NoNodeAvailableException" error message. To ensure version compatibility, we recommend that you use Transport Client 5.3.3 or [Java Low Level REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/5.5/_basic_authentication.html) to access an Elasticsearch cluster. If Transport Client is used, set `client.transport.sniff` to false. For more information, see [Transport Client \(5.x\)](/intl.en-US/Developer Guide/Java API/Transport Client (5.x).md).

## Java REST clients

Elasticsearch provides the following types of Java REST clients:

-   Java Low Level REST Client: communicates with an Elasticsearch cluster over HTTP. API operations do not encode or decode data. Java Low Level REST Client is compatible with all versions of Elasticsearch.
-   Java High Level REST Client: Java High Level REST Client is developed based on Java Low Level REST Client and is designed to expose API operation-specific methods. Java High Level REST Client depends on the Elasticsearch core project. The client accepts a request object as a parameter and returns a response object. All API operations can be synchronously or asynchronously called.
    -   The synchronous method returns a response object.
    -   The asynchronous method, whose name is suffixed with async, requires a listener parameter. This parameter notifies the target method to proceed when a response or error is received.

This topic describes how to use Java clients. We recommend that you use a REST client. Java clients include:

-   [High Level REST Client \(7.x\)](/intl.en-US/Developer Guide/Java API/High Level REST Client (7.x).md)
-   [High Level REST Client \(6.3.x\)](/intl.en-US/Developer Guide/Java API/High Level REST Client (6.3.x).md)
-   [High Level REST Client \(6.7.x\)](/intl.en-US/Developer Guide/Java API/High Level REST Client (6.7.x).md)
-   [Low Level REST Client \(5.x\)](/intl.en-US/Developer Guide/Java API/Low Level REST Client (5.x).md)
-   [Transport Client \(5.x\)](/intl.en-US/Developer Guide/Java API/Transport Client (5.x).md)

