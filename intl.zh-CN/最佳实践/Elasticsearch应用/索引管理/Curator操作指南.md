---
keyword: [安装Curator, Curator单命令行接口, crontab定时执行, 冷热数据分离实践, Curator跨节点迁移索引]
---

# Curator操作指南

Curator是Elasticsearch官方提供的一个索引管理工具，提供了删除、创建、关闭、段合并索引等功能。本文介绍Curator的使用方法，包括安装Curator、单命令行接口、crontab定时执行、冷热数据分离实践以及跨节点迁移索引。

## 安装Curator

在安装Curator前，请先完成以下准备工作：

-   创建阿里云Elasticsearch实例。

    详情请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)。

-   创建阿里云ECS实例。

    本文以CentOS 7.3 64位的ECS为例，所购买的实例需要与阿里云ES实例在同一地域和可用区，以及同一专有网络VPC（Virtual Private Cloud）下。详情请参见[使用向导创建实例](/intl.zh-CN/实例/创建实例/使用向导创建实例.md)。


连接ECS实例，执行以下命令安装Curator。

```
pip install elasticsearch-curator
```

**说明：** 建议您安装5.6.0版本的Curator，它可以支持阿里云ES 5.5.3和6.3.2版本。关于Curator版本与Elasticsearch版本的兼容性，请参见[Version Compatibility](https://www.elastic.co/guide/en/elasticsearch/client/curator/5.6/version-compatibility.html)。

安装成功后，执行以下命令查看Curator版本。

```
curator --version
```

正常情况下，返回结果如下。

```
curator, version 5.6.0
```

**说明：** 更多关于Curator的详细说明请参见[Curator](https://www.elastic.co/guide/en/elasticsearch/client/curator/index.html)。

## 单命令行接口

您可以使用curator\_cli命令执行单个操作，使用方式请参见[Singleton Command Line Interface](https://www.elastic.co/guide/en/elasticsearch/client/curator/5.6/singleton-cli.html)。

**说明：**

-   curator\_cli命令只能执行一个操作。
-   并不是所有的操作都适用于单命令行执行，例如Alias和Restore操作。

## crontab定时执行

您可以通过crontab和curator命令实现定时执行一系列操作。

curator命令格式如下。

```
curator [OPTIONS] ACTION_FILE
Options:
  --config PATH  Path to configuration file. Default: ~/.curator/curator.yml
  --dry-run      Do not perform any changes.
  --version      Show the version and exit.
  --help         Show this message and exit.
```

执行curator命令时需要指定config.yml文件和action.yml文件，详情请参见[config.yml官方文档](https://www.elastic.co/guide/en/elasticsearch/client/curator/current/configfile.html)和[action.yml官方文档](https://www.elastic.co/guide/en/elasticsearch/client/curator/current/actionfile.html)。

## 冷热数据分离实践

详细操作方法请参见[使用Curator进行冷热数据迁移](https://www.elastic.co/blog/hot-warm-architecture-in-elasticsearch-5-x)。

## 将索引从hot节点迁移到warm节点

1.  在/usr/curator/路径下创建config.yml文件，配置内容参考如下示例。

    ```
    client:
      hosts:
        - http://es-cn-0pxxxxxxxxxxxx234.elasticsearch.aliyuncs.com
      port: 9200
      url_prefix:
      use_ssl: False
      certificate:
      client_cert:
      client_key:
      ssl_no_validate: False
      http_auth: user:password
      timeout: 30
      master_only: False
    logging:
      loglevel: INFO
      logfile:
      logformat: default
      blacklist: ['elasticsearch', 'urllib3']
    ```

    -   `hosts`：替换为对应阿里云ES实例的内网或外网地址（此处以内网地址为例）。
    -   `http_auth`：替换为对应阿里云ES实例的账号和密码。
2.  在/usr/curator/路径下创建action.yml文件，配置内容参考如下示例。

    ```
    actions:
      1:
        action: allocation
        description: "Apply shard allocation filtering rules to the specified indices"
        options:
          key: box_type
          value: warm
          allocation_type: require
          wait_for_completion: true
          timeout_override:
          continue_if_exception: false
          disable_action: false
        filters:
        - filtertype: pattern
          kind: prefix
          value: logstash-
        - filtertype: age
          source: creation_date
          direction: older
          timestring: '%Y-%m-%dT%H:%M:%S'
          unit: minutes
          unit_count: 30
    ```

    以上示例按照索引创建时间，将30分钟前创建在`hot`节点以`logstash-`开头的索引迁移到`warm`节点中。您也可以根据实际场景自定义配置action.yml文件。

3.  执行以下命令，验证curator命令能否正常执行。

    ```
    curator --config /usr/curator/config.yml /usr/curator/action.yml
    ```

    正常情况下会返回类似如下所示的信息。

    ```
    2019-02-12 20:11:30,607 INFO      Preparing Action ID: 1, "allocation"
    2019-02-12 20:11:30,612 INFO      Trying Action ID: 1, "allocation": Apply shard allocation filtering rules to the specified indices
    2019-02-12 20:11:30,693 INFO      Updating index setting {'index.routing.allocation.require.box_type': 'warm'}
    2019-02-12 20:12:57,925 INFO      Health Check for all provided keys passed.
    2019-02-12 20:12:57,925 INFO      Action ID: 1, "allocation" completed.
    2019-02-12 20:12:57,925 INFO      Job completed.
    ```

4.  执行以下命令，使用crontab实现每隔15分钟定时执行curator命令。

    ```
    */15 * * * * curator --config /usr/curator/config.yml /usr/curator/action.yml
    ```


