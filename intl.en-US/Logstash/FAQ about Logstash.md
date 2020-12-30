# FAQ about Logstash

This topic provides answers to some commonly asked questions about Alibaba Cloud Logstash.

## How do I import or export data to Logstash over the Internet?

Logstash clusters are deployed in virtual private clouds \(VPCs\). You must configure NAT gateways to connect Logstash clusters to the Internet. For more information, see [Configure a NAT gateway for data transmission over the Internet](/intl.en-US/Logstash/Network and security/Configure a NAT gateway for data transmission over the Internet.md).

## When a self-managed Kafka service is used as the input or output of Logstash, the error log `No entry found for connection` is generated. What do I do?

The following code shows the error details:

```
[2019-09-22T10:01:55,914][INFO ][org.apache.kafka.clients.consumer.internals.AbstractCoordinator] [Consumer clientId=logstash-3, groupId=group_1] Discovered group coordinator iZbp15qsax98n3ho*****:9092 (id: 2147483646 rack: null)
// A few lines are omitted.
Error: No entry found for connection 2147483646
Exception: Java::JavaLang::IllegalStateException
```

Cause: The Logstash node cannot resolve the IP address that corresponds to the hostname of the Kafka service.

Solution: Add the following configuration to `server.properties`. In this example, the Kafka service runs on port 9092 of the server whose IP address is 10.10.10.10. You must replace the IP address and port number with actual values.

```
listeners=PLAINTEXT://10.10.10.10:9092
advertised.listeners=PLAINTEXT://10.10.10.10:9092
```

**Note:** We recommend that you use [Message Queue for Apache Kafka](/intl.en-US/Introduction/What is Message Queue for Apache Kafka?.md). Make sure that the IP address of the Logstash node is in the whitelist of Kafka.

## When a self-managed Kafka service is used as the input or output of Logstash, the error log `could not be established. Broker may not be available` is generated. What do I do?

Cause: The Kafka service does not exist or cannot be connected.

Solution: Check whether the Kafka service is running properly or whether the setting of `bootstrap_servers` is correct in the Logstash pipeline configuration.

## Does the Logstash JDBC driver support MySQL databases?

Yes, upload the related mysql-connector-java package. For more information, see [Configure third-party libraries](/intl.en-US/Logstash/Cluster configuration/Configure third-party libraries.md).

