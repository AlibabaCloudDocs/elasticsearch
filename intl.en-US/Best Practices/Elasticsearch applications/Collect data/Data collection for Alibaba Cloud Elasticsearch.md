# Data collection for Alibaba Cloud Elasticsearch

This topic describes the methods that are used to collect and send data from a variety of data sources to an Alibaba Cloud Elasticsearch cluster.

## Background information

Elasticsearch is widely used for data search and analytics. Developers and communities use Elasticsearch in a wide range of scenarios. The scenarios include [application search](https://www.elastic.co/app-search/), [website search](https://www.elastic.co/site-search/), logging, [infrastructure monitoring](https://www.elastic.co/infrastructure-monitoring/), [application performance monitoring \(APM\)](https://www.elastic.co/apm), and [security analytics](https://www.elastic.co/siem). Solutions for these scenarios are provided free of charge. However, before developers use these solutions, they must import the required data into Elasticsearch.

This topic provides the following common methods to collect data:

-   [Elastic Beats](#section_wxl_v1h_vbc)
-   [Logstash](#section_3di_g9p_sru)
-   [Clients](#section_q2u_yjz_uqk)
-   [Kibana](#section_q11_46v_ur6)

Elasticsearch provides a flexible RESTful API to communicate with client applications. You can call this RESTful API to collect, search for, and analyze data. You can also use the API to manage Elasticsearch clusters and indexes on the clusters.

## Elastic Beats

Elastic Beats consists of a set of lightweight data shippers that can transfer data to Elasticsearch. These shippers do not incur a number of runtime overheads. Beats can run and collect data on devices that do not have sufficient hardware resources. The devices include IoT devices, edge devices, or embedded devices. If you want to collect data but do not have sufficient resources to run a resource-intensive data shipper, we recommend that you use Beats. Based on data collected by Beats from all Internet-connected devices, you can quickly identify exceptions, such as system errors and security issues. Then, you can take measures to deal with these exceptions.

Beats can also be used in systems that have sufficient hardware resources.

You can use Beats to collect various types of data.

-   [Filebeat](https://www.elastic.co/guide/en/beats/filebeat/current/index.html)

    Filebeat can be used to read, preprocess, and transfer data from files. In most cases, you can use Filebeat to read data from log files. Filebeat can also be used to read data from non-binary files. You can use Filebeat to read data from other data sources, such as TCP, UDP, container, Redis, and syslogs. Based on various [modules](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-modules.html), Filebeat provides an easy way to collect logs of common applications, such as Apache, MySQL, and Kafka. Then, Filebeat parses the logs to obtain the required data.

-   [Metricbeat](https://www.elastic.co/guide/en/beats/metricbeat/current/index.html)

    Metricbeat can be used to collect and preprocess system and service metrics. System metrics include information about running processes, CPU utilization, memory usage, disk usage, and network usage. Based on various [modules](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-modules.html), Metricbeat can be used to collect data from various services, such as Kafka, Palo Alto Networks, and Redis.

-   [Packetbeat](https://www.elastic.co/guide/en/beats/packetbeat/current/index.html)

    Packetbeat can be used to collect and preprocess real-time network data. You can use Packetbeat for security analytics, application monitoring, and performance analytics. Packetbeat supports the following protocols: DHCP, DNS, HTTP, MongoDB, NFS, and TLS.

-   [Winlogbeat](https://www.elastic.co/guide/en/beats/winlogbeat/current/index.html)

    Winlogbeat can be used to capture event logs from Windows operating systems. The event logs include application, hardware, security, and system events.

-   [Auditbeat](https://www.elastic.co/guide/en/beats/auditbeat/current/index.html)

    Auditbeat can be used to detect changes to critical files and collect audit events from the Linux audit framework. In most cases, Auditbeat is used for security analytics.

-   [Heartbeat](https://www.elastic.co/guide/en/beats/heartbeat/current/index.html)

    Heartbeat can be used to check the availability of your system and services by probing. Heartbeat applies to many scenarios, such as infrastructure monitoring and security analytics. Heartbeat supports ICMP, TCP, and HTTP.

-   [Functionbeat](https://www.elastic.co/guide/en/beats/functionbeat/current/index.html)

    Functionbeat can be used to collect logs and metrics from serverless environments such as AWS Lambda.


For more information about how to use Metricbeat, see [Use user-created Metricbeat to collect system metrics](/intl.en-US/Best Practices/Elasticsearch applications/Collect data/Use user-created Metricbeat to collect system metrics.md). Use other shippers in a similar way.

## Logstash

Logstash is a powerful and flexible tool that is used to read, process, and transfer all types of data. Logstash provides a variety of features and has high requirements for device performance. Beats does not support some features provided by Logstash, or it is costly to use Beats for some features. For example, it is costly to use Beats to enrich documents by searching for data in external data sources. Logstash has higher requirements for hardware resources than Beats. Therefore, Logstash cannot be deployed on devices whose hardware resources cannot meet the minimum requirements. If Beats is not qualified for specific scenarios, use Logstash instead.

In most cases, Beats and Logstash work collaboratively. Specifically, use Beats to collect data and Logstash to process data.

Alibaba Cloud Elasticsearch integrates the Logstash service. Alibaba Cloud Logstash is a server-side data processing pipeline. It is compatible with all the capabilities of open source Logstash. Alibaba Cloud Logstash can be used to dynamically collect data from multiple data sources at the same time and transform and store collected data to a specified location. Alibaba Cloud Logstash can be used to process and transform all types of events by using input, filter, and output plug-ins.

Logstash data processing pipelines are used to run tasks. Each pipeline consists of at least one input plug-in, one filter plug-in, and one output plug-in.

-   [Input plug-ins](https://www.elastic.co/guide/en/logstash/current/input-plugins.html)

    Input plug-ins can be used to read data from different data sources. The supported data sources include files, HTTP, IMAP, JDBC, Kafka, syslogs, TCP, and UDP.

-   [Filter plug-ins](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html)

    Filter plug-ins can process and enrich data by using multiple methods. In most cases, filter plug-ins first parse unstructured log data and transform the data into structured data. Logstash provides the [Grok filter plug-in](https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html) to parse regular expressions, CSV data, JSON data, key-value pairs, delimited unstructured data, and complex unstructured data. Logstash also provides other filter plug-ins to enrich data. The plug-ins are used to query [DNS records](https://www.elastic.co/guide/en/logstash/current/plugins-filters-dns.html), add [the locations of IP addresses](https://www.elastic.co/guide/en/logstash/current/plugins-filters-geoip.html), or search for [custom directories](https://www.elastic.co/guide/en/logstash/current/plugins-filters-translate.html) or [Elasticsearch indexes](https://www.elastic.co/guide/en/logstash/current/plugins-filters-elasticsearch.html). Additional filter plug-ins, such as [mutate](https://www.elastic.co/guide/en/logstash/current/plugins-filters-mutate.html), allow you to perform diverse data transformations. For example, you can rename, delete, and copy data fields and values.

-   [Output plug-ins](https://www.elastic.co/guide/en/logstash/current/output-plugins.html)

    Output plug-ins can be used to write the parsed and enriched data into data sinks. These plug-ins are used in the final stage of Logstash pipelines. Multiple types of output plug-ins are available. However, this topic focuses on the [Elasticsearch output plug-in](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-elasticsearch.html). This plug-in can be used to send data collected from a variety of data sources to an Elasticsearch cluster.


The following section describes a sample Logstash pipeline. It can be used to complete the following operations:

-   Read the Elastic Blogs RSS feed.
-   Preprocess the data by copying or renaming fields and removing special characters and HTML tags.
-   Send the data to Elasticsearch.

1.  Configure a Logstash pipeline.

    ```
    input { 
      rss { 
        url => "/blog/feed" 
        interval => 120 
      } 
    } 
    filter { 
      mutate { 
        rename => [ "message", "blog_html" ] 
        copy => { "blog_html" => "blog_text" } 
        copy => { "published" => "@timestamp" } 
      } 
      mutate { 
        gsub => [  
          "blog_text", "<.*?>", "",
          "blog_text", "[\n\t]", " " 
        ] 
        remove_field => [ "published", "author" ] 
      } 
    } 
    output { 
      stdout { 
        codec => dots 
      } 
      elasticsearch { 
        hosts => [ "https://<your-elsaticsearch-url>" ] 
        index => "elastic_blog" 
        user => "elastic" 
        password => "<your-elasticsearch-password>" 
      } 
    }
    ```

    Set `hosts` to a value in the format of `<Internal endpoint of your Elasticsearch cluster>:9200`. Set `password` to the password that is used to access the Elasticsearch cluster.

2.  In the Kibana console, view the migrated index data.

    ```
    POST elastic_blog/_search
    ```


## Clients

You can use Elasticsearch clients to integrate data collection code with tailored application code. These clients are libraries that abstract low-level details of the data collection. They allow you to focus on specific operations that are related to your application. Elasticsearch supports multiple programming languages for clients, such as Java, JavaScript, Go, .NET, PHP, Perl, Python, and Ruby. For more information about the programming languages and the details and sample code of your selected language, see [Elasticsearch Clients](https://www.elastic.co/guide/en/elasticsearch/client/index.html).

If the programming language of your application is not included in the preceding supported languages, obtain the required information from [Community Contributed Clients](https://www.elastic.co/guide/en/elasticsearch/client/community/current/index.html).

## Kibana

We recommend that you use the [Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md) to develop and debug Elasticsearch requests. Kibana provides all features of the RESTful API in Elasticsearch and abstracts the technical details of underlying HTTP requests. You can use Kibana to add original JSON documents to an Elasticsearch cluster.

```
PUT my_first_index/_doc/1 
{ 
    "title" :"How to Ingest Into Elasticsearch Service",
    "date" :"2019-08-15T14:12:12",
    "description" :"This is an overview article about the various ways to ingest into Elasticsearch Service" 
}
```

**Note:** In addition to Kibana, you can use other tools to communicate with Elasticsearch and collect documents by calling the RESTful API. For example, you can use [cURL](https://github.com/curl/curl) to develop and debug Elasticsearch requests or integrate tailored scripts.

## Summary

Multiple methods are provided to collect and send data from a variety of data sources to Elasticsearch. You must select the most suitable method based on your business scenarios, requirements, and operating systems.

-   Beats data shippers are convenient, lightweight, and out-of-the-box. They can be used to collect data from various data sources. Modules that are packaged with Beats provide the configurations of data acquisition, parsing, indexing, and visualization for many common databases, operating systems, containers, web servers, and caches. These modules allow you to create a dashboard for your data within five minutes. Beats data shippers are most suited to embedded devices that do not have sufficient resources, such as IoT devices or firewalls.
-   Logstash is a flexible tool to read, transform, and transfer data. It provides various input, filter, and output plug-ins. If Beats cannot meet the requirements for specific scenarios, you can use Beats to collect data, use Logstash to process data, and then transfer the processed data to Elasticsearch.
-   To collect data from applications, we recommend that you use clients that are supported by open source Elasticsearch.
-   To develop or debug Elasticsearch requests, we recommend that you use Kibana.

## References

-   [How to ingest data into Elasticsearch Service](https://www.elastic.co/cn/blog/how-to-ingest-data-into-elasticsearch-service)
-   [Should I use Logstash or Elasticsearch ingest nodes?](https://www.elastic.co/cn/blog/should-i-use-logstash-or-elasticsearch-ingest-nodes)
-   [Get System Logs and Metrics into Elasticsearch with Beats System Modules](https://www.elastic.co/cn/blog/get-system-logs-and-metrics-into-elasticsearch-with-beats-system-modules)

