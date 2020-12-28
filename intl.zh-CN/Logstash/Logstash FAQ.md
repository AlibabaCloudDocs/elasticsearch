# Logstash FAQ

本文介绍使用阿里云Logstash的常见问题。

## Logstash是否支持将数据源配置为DRDS？

支持。可参考RDS MySQL数据迁移方案进行配置，具体操作步骤请参见[通过Logstash将RDS MySQL数据同步至Elasticsearch]()。

## 如何将公网数据导入或导出到Logstash中？

Logstash实例部署在专有网络VPC（Virtual Private Cloud）下，可以通过配置NAT网关实现与公网的连通，详情请参见[配置NAT公网数据传输](/intl.zh-CN/Logstash/网络与安全/配置NAT公网数据传输.md)。

## 使用自建Kafka作为Logstash的输入或者输出时，出现如下`No entry found for connection`错误日志，如何处理？

详细错误信息如下。

```
[2019-09-22T10:01:55,914][INFO ][org.apache.kafka.clients.consumer.internals.AbstractCoordinator] [Consumer clientId=logstash-3, groupId=group_1] Discovered group coordinator iZbp15qsax98n3ho*****:9092 (id: 2147483646 rack: null)
// 省略若干行日志
Error: No entry found for connection 2147483646
Exception: Java::JavaLang::IllegalStateException
```

原因：Logstash节点无法解析到Kafka服务的hostname对应的IP地址。

解决方法：请在`server.properties`中添加如下配置（假设Kafka服务运行在10.10.10.10的9092端口，需要替换为您自己的IP地址和端口号）。

```
listeners=PLAINTEXT://10.10.10.10:9092
advertised.listeners=PLAINTEXT://10.10.10.10:9092
```

**说明：** 推荐您使用[阿里云Kafka服务](/intl.zh-CN/产品简介/什么是消息队列Kafka版？.md)，并且需要保证Logstash所在节点的IP地址在Kafka的访问白名单内。

## 使用自建Kafka作为Logstash的输入或者输出时，出现`could not be established. Broker may not be available`错误日志，如何处理？

原因：Kafka服务不存在或者无法连接。

解决方法：请检查Kafka服务是否正常运行，或者Logstash管道配置中的`bootstrap_servers`配置是否正确。

## 阿里云Logstash的JDBC支持MySQL数据库吗？

支持。请参见[配置扩展文件](/intl.zh-CN/Logstash/集群配置/配置扩展文件.md)上传对应的mysql-connector-java包。

