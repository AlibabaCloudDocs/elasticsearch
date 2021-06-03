# Logstash默认插件列表

本文介绍阿里云Logstash实例默认集成的插件。

**说明：**

-   当[input](https://www.elastic.co/guide/en/logstash/7.4/input-plugins.html)插件需要监听Logstash所在机器的端口时，需要使用8000～9000之间的端口。
-   阿里云Logstash不支持[input](https://www.elastic.co/guide/en/logstash/7.4/input-plugins.html)的file插件，如有需求，可以使用Filebeat作为本地文件的采集器，以及Logstash的输入源。

|类别|名称|说明|介绍|
|--|--|--|--|
|input|azure\_event\_hubs|使用Azure事件中心中的事件。|[Azure Event Hubs plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-azure_event_hubs.html)|
|beats|从Elastic Beats框架中接收事件。|[Beats input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-beats.html)|
|dead\_letter\_queue|从Logstash的死信队列中读取事件。|[Dead\_letter\_queue input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-dead_letter_queue.html)|
|elasticsearch|从Elasticsearch集群中读取数据。|[Elasticsearch input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-elasticsearch.html)|
|exec|定期运行Shell命令，将shell命令的全部输出作为事件捕获。|[Exec input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-exec.html)|
|ganglia|通过用户数据协议UDP（User Datagram Protocol）从网络读取Ganglia包。|[Ganglia input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-ganglia.html)|
|gelf|在网络上将GELF格式信息作为事件读取。|[Gelf input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-gelf.html)|
|generator|生成随机日志事件。|[Generator input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-generator.html)|
|graphite|从Graphite工具读取指标。|[Graphite input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-graphite.html)|
|heartbeat|生成心跳消息。|[Heartbeat input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-heartbeat.html)|
|http|通过HTTP或HTTPS接收单行或多行事件。|[Http input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-http.html)|
|http\_poller|调用HTTP API，将输出解码为事件，并发送事件。|[Http\_poller input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-http_poller.html)|
|imap|从IMAP服务器读取邮件。|[Imap input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-imap.html)|
|jdbc|通过JDBC程序界面，将任一数据库数据读取到Logstash中。|[Jdbc input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-jdbc.html)|
|kafka|从Kafka主题读取事件。|[Kafka input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-kafka.html)|
|pipe|从长时间运行的管道命令中流式读取事件。|[Pipe input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-pipe.html)|
|rabbitmq|从RabbitMQ队列中读取事件。|[Rabbitmq input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-rabbitmq.html)|
|redis|从Redis实例中读取事件。|[Redis input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-redis.html)|
|s3|从S3 Bucket中的文件流式读取事件。|[S3 input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-s3.html)|
|snmp|使用简单网络管理协议（SNMP）轮询网络设备，获取当前设备操作状态信息。|[SNMP input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-snmp.html)|
|snmptrap|将SNMP trap消息作为事件读取。|[Snmptrap input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-snmptrap.html)|
|sqs|从Amazon Web Services简单队列服务（Simple Queue Service, SQS）队列中读取事件。|[Sqs input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-sqs.html)|
|stdin|从标准输入读取事件。|[Stdin input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-stdin.html)|
|syslog|在网络上将syslog消息作为事件读取。|[Syslog input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-syslog.html)|
|tcp|通过TCP套接字读取事件。|[Tcp input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-tcp.html)|
|twitter|从Twitter Streaming API接收事件。|[Twitter input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-twitter.html)|
|udp|在网络上通过UDP，将消息作为事件读取。|[Udp input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-udp.html)|
|unix|通过UNIX套接字读取事件。|[Unix input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-unix.html)|
|output|kafka|向Kafka主题写入事件。|[Kafka output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-kafka.html)|
|lumberjack|使用lumberjack协议发送事件。|[Lumberjack output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-lumberjack.html)|
|nagios|通过Nagios命令文件，向Nagios发送被动检查结果。|[Nagios output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-nagios.html)|
|pagerduty|根据预先配置的服务和升级政策发送通知。|[Pagerduty output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-pagerduty.html)|
|pipe|将事件输送到另一个程序的标准输入。|[Pipe output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-pipe.html)|
|rabbitmq|将事件推送到RabbitMQ exchange。|[Rabbitmq output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-rabbitmq.html)|
|redis|使用RPUSH命令将事件发送到Redis队列。|[Redis output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-redis.html)|
|s3|向亚马逊简单存储服务（Amazon Simple Storage Service, Amazon S3）批量上传Logstash事件。|[S3 output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-s3.html)|
|sns|向采用托管pub/sub框架的亚马逊简单通知服务（Amazon Simple Notification Service）发送事件。|[Sns output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-sns.html)|
|sqs|将事件推送到Amazon Web Services（AWS）SQS队列。|[Sqs output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-sqs.html)|
|stdout|将事件打印到运行shell命令的Logstash标准输出。|[Stdout output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-stdout.html)|
|tcp|通过TCP套接字写入事件。|[Tcp output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-tcp.html)|
|udp|通过UDP发送事件。|[Udp output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-udp.html)|
|webhdfs|通过webhdfs REST API向HDFS中的文件发送Logstash事件。|[Webhdfs output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-webhdfs.html)|
|cloudwatch|聚合并发送指标数据到AWS CloudWatch。|[Cloudwatch output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-cloudwatch.html)|
|csv|以逗号分隔（CSV）或其他分隔的格式，将事件写入磁盘。基于文件输出共享配置值。内部使用Ruby CSV库。|[Csv output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-csv.html)|
|elastic\_app\_search|向Elastic App Search解决方案发送事件。|[App Search output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-elastic_app_search.html)|
|email|收到输出后发送电子邮件。 您也可以使用条件来包含，或者排除电子邮件输出执行。|[Email output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-email.html)|
|file|向磁盘文件写入事件。您可以将事件中的字段作为文件名和/或路径的一部分。|[File output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-file.html)|
|graphite|从日志中读取指标数据，并将它们发送到Graphite工具。Graphite是一个用于存储和绘制指标的开源工具。|[Graphite output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-graphite.html)|
|http|向通用HTTP或HTTPS端点发送事件。|[Http output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-http.html)|
|filter|aggregate|聚合单个任务下多个事件（通常为日志记录）的信息，并将聚合信息推送到最后的任务事件。|[Aggregate filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-aggregate.html)|
|anonymize|将字段值替换为一致性哈希值，以实现字段匿名化。|[Anonymize filter plugin](https://www.elastic.co/guide/en/logstash/5.5/plugins-filters-anonymize.html)|
|cidr|根据网络块列表检查事件中的IP地址。|[Cidr filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-cidr.html)|
|clone|检查重复事件。将为克隆列表中的每个类型创建克隆。|[Clone filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-clone.html)|
|csv|解析包含CSV数据的事件字段，并将其作为单独字段存储（名字也可指定）。 该过滤器还可以解析带有任何分隔符（不只是逗号）的数据。|[Csv filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-csv.html)|
|date|解析字段中的日期，然后使用该日期或时间戳作为事件的logstash时间戳。|[Date filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-date.html)|
|de\_dot|将“.”字符替换为其他分隔符，以重命名字段。 在实际应用中，该过滤器的代价较大。它必须将源字段内容拷贝到新目的字段（该新字段的名字中不再包含点，“.”），然后移除相应源字段。|[De\_dot filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-de_dot.html)|
|dissect|Dissect过滤器是一种拆分操作。|[Dissect filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-dissect.html)|
|dns|对“reverse”阵列下的各个或指定记录执行DNS查找（A记录或CNAME记录查找，或PTR记录的反向查找）。|[Dns filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-dns.html)|
|drop|删除满足此过滤器的所有事件。|[Drop filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-drop.html)|
|elasticsearch|搜索Elasticsearch中的过往日志事件，并将其部分字段复制到当前事件。|[Elasticsearch filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-elasticsearch.html)|
|fingerprint|创建一个或多个字段的一致性哈希值（指纹），并将结果存储在新字段。|[Fingerprint filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-fingerprint.html)|
|geoip|根据Maxmind GeoLite2数据库的数据添加关于IP地址的地理位置信息。|[Geoip filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-geoip.html)|
|grok|解析任意非结构化文本并将其结构化。|[Grok filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-grok.html)|
|http|整合外部网络服务或多个REST API。|[HTTP filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-http.html)|
|jdbc\_static|使用预先从远程数据库加载的数据丰富事件。|[Jdbc\_static filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-jdbc_static.html)|
|jdbc\_streaming|执行SQL查询，并将结果集存储到“目标”字段。 将结果缓存到具有有效期的本地最近最少使用（LRU）缓存。|[Jdbc\_streaming filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-jdbc_streaming.html)|
|json|JSON解析过滤器，将包含JSON的已有字段扩展为Logstash事件中的实际数据结构。|[JSON filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-json.html)|
|kv|自动解析各种“foo=bar”消息（或特定事件字段）。|[Kv filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-kv.html)|
|memcached|将外部数据整合到Memcached。|[Memcached filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-memcached.html)|
|metrics|聚合指标。|[Metrics filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-metrics.html)|
|mutate|在字段上执行转变。您可以重命名、删除、更换并修改您事件中的字段。|[Mutate filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-mutate.html)|
|ruby|执行Ruby代码。 该过滤器接受内联Ruby代码或文件。 这两个方案互斥，在工作方式方面略有不同。|[Ruby filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-ruby.html)|
|sleep|按照指定的休眠时间长度休眠。 在休眠时间内，Logstash将停止。这有助于限流。|[Sleep filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-sleep.html)|
|split|通过分解事件的一个字段，并将产生的每个值嵌入原始事件的克隆版，从而克隆事件。 被分解的字段可以是字符串或字符串数组。|[Split filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-split.html)|
|syslog\_pri|解析Syslog（RFC3164）消息前部的“PRI”字段。 如果没有设置优先级，它会默认为13（每个请求注解）。|[Syslog\_pri filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-syslog_pri.html)|
|throttle|限制事件的数量。|[Throttle filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-throttle.html)|
|translate|一款普通搜索和替换工具，基于配置的哈希和/或文件决定替换值。|[Translate filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-translate.html)|
|truncate|截断超过一定长度的字段。|[Truncate filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-truncate.html)|
|urldecode|解码URL编码字段。|[Urldecode filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-urldecode.html)|
|useragent|基于BrowserScope数据，将用户代理字符串解析为结构化数据。|[Useragent filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-useragent.html)|
|xml|XML过滤器。将包含XML的字段，扩展为实际数据结构。|[Xml filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-xml.html)|
|codec|cef|根据《实施 ArcSight 通用事件格式》第二十次修订版（2013 年 6 月 5 日），使用Logstash编解码器处理ArcSight通用事件格式（CEF）。|[Cef codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-cef.html)|
|collectd|在网络上通过UDP从collectd二进制协议中读取事件。|[Collectd codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-collectd.html)|
|dots|该编解码器生成一个点（.），代表它处理的一个事件。|[Dots codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-dots.html)|
|edn|读取并产生EDN格式数据。|[Edn codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-edn.html)|
|edn\_lines|读取并产生以新行分隔的EDN格式数据。|[Edn\_lines codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-edn_lines.html)|
|es\_bulk|将Elasticsearch bulk格式解码到单独的事件中，并将元数据解码到\[@metadata\]\(/metadata\)字段。|[Es\_bulk codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-es_bulk.html)|
|fluent|处理fluentd msgpack模式。|[Fluent codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-fluent.html)|
|graphite|编码并解码Graphite格式的行。|[Graphite codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-graphite.html)|
|json|解码（通过输入）和编码（通过输出）完整的JSON消息。|[Json codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-json.html)|
|json\_lines|解码以新行分隔的JSON流。|[Json\_lines codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-json_lines.html)|
|line|读取面向行的文本数据。|[Line codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-line.html)|
|msgpack|读取并产生MessagePack编码内容。|[Msgpack codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-msgpack.html)|
|multiline|将多行消息合并到单个事件中。|[Multiline codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-multiline.html)|
|netflow|解码Netflow v5、v9和v10（IPFIX）数据。|[Netflow codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-netflow.html)|
|plain|处理事件之间没有分隔的明文。|[Plain codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-plain.html)|
|rubydebug|使用Ruby Awesome Print库输出Logstash事件数据。|[Rubydebug codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-rubydebug.html)|

