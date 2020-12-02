# Built-in plug-ins of Logstash

This topic describes the built-in plug-ins of Alibaba Cloud Logstash.

**Note:**

-   The [input](https://www.elastic.co/guide/en/logstash/7.4/input-plugins.html) plug-ins can listen to only the ports from 8000 to 9000 of the server where Alibaba Cloud Logstash resides.
-   Alibaba Cloud Logstash does not support the file plug-in under [input](https://www.elastic.co/guide/en/logstash/7.4/input-plugins.html) of open-source Logstash. If you require this type of plug-in, we recommend that you use Filebeat as the local file collector and the input source for Logstash.

|Category|Plug-in|Description|Reference|
|--------|-------|-----------|---------|
|input|azure\_event\_hubs|Consumes events from Azure Event Hubs.|[Azure Event Hubs plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-azure_event_hubs.html)|
|beats|Receives events from the Elastic Beats framework.|[Beats input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-beats.html)|
|dead\_letter\_queue|Reads events from the dead-letter queue of Logstash.|[Dead\_letter\_queue input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-dead_letter_queue.html)|
|elasticsearch|Reads query results from an Elasticsearch cluster.|[Elasticsearch input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-elasticsearch.html)|
|exec|Runs a shell command periodically and captures the output of the shell command as an event.|[Exec input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-exec.html)|
|ganglia|Reads Ganglia packets over User Datagram Protocol \(UDP\).|[Ganglia input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-ganglia.html)|
|gelf|Reads Graylog Extended Log Format \(GELF\) messages from networks as events.|[Gelf input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-gelf.html)|
|generator|Generates random log events.|[Generator input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-generator.html)|
|graphite|Reads metrics from Graphite.|[Graphite input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-graphite.html)|
|heartbeat|Generates heartbeat messages.|[Heartbeat input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-heartbeat.html)|
|http|Receives single-line or multiline events over HTTP or HTTPS.|[Http input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-http.html)|
|http\_poller|Decodes the output of an HTTP API into events and sends the events.|[Http\_poller input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-http_poller.html)|
|imap|Reads emails from an Internet Message Access Protocol \(IMAP\) server.|[Imap input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-imap.html)|
|jdbc|Reads data from a database with a Java Database Connectivity \(JDBC\) interface into Logstash.|[Jdbc input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-jdbc.html)|
|kafka|Reads events from a Kafka topic.|[Kafka input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-kafka.html)|
|pipe|Streams events from a long-running command pipe.|[Pipe input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-pipe.html)|
|rabbitmq|Reads events from a RabbitMQ queue.|[Rabbitmq input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-rabbitmq.html)|
|redis|Reads events from a Redis instance.|[Redis input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-redis.html)|
|s3|Streams events from the objects in an Amazon Simple Storage Service \(Amazon S3\) bucket.|[S3 input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-s3.html)|
|snmp|Polls network devices by using Simple Network Management Protocol \(SNMP\) to obtain the status of the devices.|[SNMP input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-snmp.html)|
|snmptrap|Reads SNMP trap messages as events.|[Snmptrap input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-snmptrap.html)|
|sqs|Reads events from an Amazon Simple Queue Service \(SQS\) queue.|[Sqs input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-sqs.html)|
|stdin|Reads events from standard input.|[Stdin input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-stdin.html)|
|syslog|Reads Syslog messages from networks as events.|[Syslog input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-syslog.html)|
|tcp|Reads events over a TCP socket.|[Tcp input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-tcp.html)|
|twitter|Reads events from the Twitter Streaming API.|[Twitter input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-twitter.html)|
|udp|Reads messages from networks over UDP as events.|[Udp input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-udp.html)|
|unix|Reads events over a UNIX socket.|[Unix input plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-inputs-unix.html)|
|output|kafka|Writes events to a Kafka topic.|[Kafka output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-kafka.html)|
|lumberjack|Sends events by using the lumberjack protocol.|[Lumberjack output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-lumberjack.html)|
|nagios|Sends passive check results to Nagios by using Nagios command files.|[Nagios output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-nagios.html)|
|pagerduty|Sends notifications based on preconfigured services and upgrade policies.|[Pagerduty output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-pagerduty.html)|
|pipe|Pushes events to the standard input of another program by using pipes.|[Pipe output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-pipe.html)|
|rabbitmq|Pushes events to a RabbitMQ exchange.|[Rabbitmq output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-rabbitmq.html)|
|redis|Sends events to a Redis queue by using the RPUSH command.|[Redis output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-redis.html)|
|s3|Uploads Logstash events to Amazon S3 at a time.|[S3 output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-s3.html)|
|sns|Sends events to Amazon Simple Notification Service \(SNS\), a fully managed pub/sub messaging service.|[Sns output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-sns.html)|
|sqs|Pushes events to an Amazon SQS queue.|[Sqs output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-sqs.html)|
|stdout|Returns events to the standard output of Logstash that runs shell commands.|[Stdout output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-stdout.html)|
|tcp|Writes events over a TCP socket.|[Tcp output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-tcp.html)|
|udp|Sends events over UDP.|[Udp output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-udp.html)|
|webhdfs|Sends Logstash events to the files in Hadoop Distributed File System \(HDFS\) by calling the WebHDFS RESTful API.|[Webhdfs output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-webhdfs.html)|
|cloudwatch|Aggregates and sends metrics to Amazon CloudWatch.|[Cloudwatch output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-cloudwatch.html)|
|csv|Writes events to disks in CSV or a delimited format. This plug-in is based on the file output, and many configuration values are shared. The Ruby CSV library is not recommended in the production environment.|[Csv output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-csv.html)|
|elastic\_app\_search|Sends events to Elastic App Search.|[App Search output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-elastic_app_search.html)|
|email|Sends an email when output is received. You can include or exclude the email output execution by using conditions.|[Email output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-email.html)|
|file|Writes events to files on disks. You can use the fields from an event as a part of the file name or path.|[File output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-file.html)|
|graphite|Reads metrics from logs and sends the metrics to Graphite. Graphite is an open-source tool that allows you to store and graph metrics.|[Graphite output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-graphite.html)|
|http|Sends events to a universal HTTP or HTTPS endpoint.|[Http output plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-outputs-http.html)|
|filter|aggregate|Aggregates information from several events that are typically log records and belong to the same task, and then pushes the aggregated information to the final task event.|[Aggregate filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-aggregate.html)|
|anonymize|Anonymizes fields by replacing field values with a consistent hash.|[Anonymize filter plugin](https://www.elastic.co/guide/en/logstash/5.5/plugins-filters-anonymize.html)|
|cidr|Checks IP addresses in events against a list of network blocks.|[Cidr filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-cidr.html)|
|clone|Duplicates events. This plug-in creates a clone for each type in the clone list.|[Clone filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-clone.html)|
|csv|Parses an event field that contains CSV data, and stores the field as an individual field. You can also specify the field name. This plug-in still parses data with delimiters, which include but are not limited to commas \(,\).|[Csv filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-csv.html)|
|date|Parses a date from fields and then uses that date or timestamp as the Logstash timestamp for the event.|[Date filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-date.html)|
|de\_dot|Replaces the dot \(.\) with a different delimiter to rename a field. This plug-in is not cost effective for practical use. It copies the content of a source field to a destination field whose name no longer contains dots, and then removes the source field.|[De\_dot filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-de_dot.html)|
|dissect|Performs a split operation.|[Dissect filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-dissect.html)|
|dns|Performs a lookup on specified or all records under the reverse arrays. The lookup is either an A or CNAME record lookup or a reverse lookup on PTR records.|[Dns filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-dns.html)|
|drop|Drops all events that meet filter conditions.|[Drop filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-drop.html)|
|elasticsearch|Searches Elasticsearch for a historical log event and copies some fields from the event to the current event.|[Elasticsearch filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-elasticsearch.html)|
|fingerprint|Creates consistent hashes as fingerprints for one or more fields and stores the results in a new field.|[Fingerprint filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-fingerprint.html)|
|geoip|Adds the geographical locations of IP addresses based on the data in Maxmind GeoLite2 databases.|[Geoip filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-geoip.html)|
|grok|Parses arbitrary text that is unstructured data into structured data.|[Grok filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-grok.html)|
|http|Provides integration with external web services or RESTful APIs.|[HTTP filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-http.html)|
|jdbc\_static|Enriches events with data that is preloaded from a remote database.|[Jdbc\_static filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-jdbc_static.html)|
|jdbc\_streaming|Executes an SQL query and stores the result set in the field that is specified as a target. This plug-in locally caches results in the Least Recently Used \(LRU\) cache that is valid.|[Jdbc\_streaming filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-jdbc_streaming.html)|
|json|Expands an existing field that contains JSON data into an actual data structure within a Logstash event. This plug-in is a JSON parsing filter.|[JSON filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-json.html)|
|kv|Enables automatic parsing of messages or specific event fields. The messages are foo=bar metasyntactic variables.|[Kv filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-kv.html)|
|memcached|Provides integration with external data in Memcached.|[Memcached filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-memcached.html)|
|metrics|Aggregates metrics.|[Metrics filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-metrics.html)|
|mutate|Performs mutations on fields. You can rename, remove, replace, and modify fields in your events.|[Mutate filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-mutate.html)|
|ruby|Executes Ruby code. This plug-in accepts inline Ruby code or a Ruby file. The two options are mutually exclusive and are slightly different in the methods of working.|[Ruby filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-ruby.html)|
|sleep|Enters the sleep mode for a specified time range. This causes Logstash to stall in this period, which facilitates throttling.|[Sleep filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-sleep.html)|
|split|Clones an event by splitting one of its fields and placing each value resulting from the split operation into a clone of the original event. The field for splitting can be either a string or an array.|[Split filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-split.html)|
|syslog\_pri|Parses the PRI field of a Syslog message. PRI is short for priority. For more information about the Syslog messages, see RFC 3164. If no priority is set, the default value is 13 per RFC.|[Syslog\_pri filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-syslog_pri.html)|
|throttle|Limits the number of events.|[Throttle filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-throttle.html)|
|translate|Uses a configured hash or a file to determine replacement values. This plug-in is a general search and replacement tool.|[Translate filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-translate.html)|
|truncate|Truncates fields that exceed a specified length.|[Truncate filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-truncate.html)|
|urldecode|Decodes URL-encoded fields.|[Urldecode filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-urldecode.html)|
|useragent|Parses user agent strings into structured data based on Browserscope data.|[Useragent filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-useragent.html)|
|xml|Expands a field that contains XML data into an actual data structure. This plug-in is an XML filter.|[Xml filter plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-filters-xml.html)|
|codec|cef|Reads data in ArcSight Common Event Format \(CEF\). This plug-in is an implementation of a Logstash codec. It is based on Revision 20 of Implementing ArcSight CEF, dated from June 5, 2013.|[Cef codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-cef.html)|
|collectd|Reads events on networks from the collectd binary protocol over UDP.|[Collectd codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-collectd.html)|
|dots|Generates a dot \(.\) to represent each event that is processed by this plug-in.|[Dots codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-dots.html)|
|edn|Reads and produces data in the Extensible Data Notation \(EDN\) format.|[Edn codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-edn.html)|
|edn\_lines|Reads and produces EDN-formatted data that is delimited by line breaks.|[Edn\_lines codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-edn_lines.html)|
|es\_bulk|Decodes the Elasticsearch bulk format into individual events and decodes metadata into the \[@metadata\]\(/metadata\) field.|[Es\_bulk codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-es_bulk.html)|
|fluent|Handles the MessagePack schema for Fluentd.|[Fluent codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-fluent.html)|
|graphite|Encodes and decodes Graphite-formatted lines.|[Graphite codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-graphite.html)|
|json|Decodes and encodes full JSON messages. The decoding process is based on inputs, and the encoding process is based on outputs.|[Json codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-json.html)|
|json\_lines|Decodes streamed JSON data that is delimited by line breaks.|[Json\_lines codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-json_lines.html)|
|line|Reads line-oriented text data.|[Line codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-line.html)|
|msgpack|Reads and produces MessagePack-encoded content.|[Msgpack codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-msgpack.html)|
|multiline|Merges multiline messages into a single event.|[Multiline codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-multiline.html)|
|netflow|Decodes Netflow v5, v9, and v10 \(IPFIX\) flows.|[Netflow codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-netflow.html)|
|plain|Handles the plaintext with no delimiters between events.|[Plain codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-plain.html)|
|rubydebug|Generates Logstash event data by using the Ruby Awesome Print library.|[Rubydebug codec plugin](https://www.elastic.co/guide/en/logstash/6.7/plugins-codecs-rubydebug.html)|

