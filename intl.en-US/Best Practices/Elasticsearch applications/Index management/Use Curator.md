---
keyword: [install Curator, Curator singleton CLI, schedule a task by using crontab, separate hot and cold data, migrate indexes from hot nodes to warm nodes]
---

# Use Curator

Curator is an index management tool provided by open source Elasticsearch. This tool allows you to create, delete, and disable indexes. It also allows you to merge index segments. This topic describes how to install Curator and what features are provided. You can use Curator to run the singleton command line interface \(CLI\), schedule a task by using crontab, separate hot and cold data, and migrate indexes from hot nodes to warm nodes.

## Install Curator

Before you install Curator, make sure that you have completed the following operations:

-   Create an Alibaba Cloud Elasticsearch cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md).

-   Create an Alibaba Cloud Elastic Compute Service \(ECS\) instance.

    An ECS instance that runs 64-bit CentOS 7.3 is used in this topic. The created ECS instance must reside in the same region, zone, and virtual private cloud \(VPC\) as the Elasticsearch cluster. For more information, see [Create an instance by using the wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the wizard.md).


Connect to the ECS instance and run the following command to install Curator:

```
pip install elasticsearch-curator
```

**Note:** We recommend that you install Curator 5.6.0. This version supports Alibaba Cloud Elasticsearch V5.5.3 and V6.3.2 clusters. For more information about the compatibility between Curator versions and Alibaba Cloud Elasticsearch versions, see [Version Compatibility](https://www.elastic.co/guide/en/elasticsearch/client/curator/5.6/version-compatibility.html).

After Curator is installed, run the following command to check its version:

```
curator --version
```

If the operation is successful, the following result is returned:

```
curator, version 5.6.0
```

**Note:** For more information about Curator, see [Curator](https://www.elastic.co/guide/en/elasticsearch/client/curator/index.html).

## Use the singleton CLI

You can run the curator\_cli command to perform a single operation. For more information, see [Singleton Command Line Interface](https://www.elastic.co/guide/en/elasticsearch/client/curator/5.6/singleton-cli.html).

**Note:**

-   The curator\_cli command allows you to perform only one operation at a time.
-   Some operations such as Alias and Restore cannot be performed by using the singleton CLI.

## Schedule a task by using crontab

You can use the crontab and curator commands to schedule the operations of a task.

The following code provides an example of the curator command:

```
curator [OPTIONS] ACTION_FILE
Options:
  --config PATH  Path to configuration file. Default: ~/.curator/curator.yml
  --dry-run      Do not perform any changes.
  --version      Show the version and exit.
  --help         Show this message and exit.
```

Before you run the curator command, you must specify the [config.yml](https://www.elastic.co/guide/en/elasticsearch/client/curator/current/configfile.html) and [action.yml](https://www.elastic.co/guide/en/elasticsearch/client/curator/current/actionfile.html) files.

## Separate hot and cold data

For more information, see ["Hot-Warm" Architecture in Elasticsearch 5.x](https://www.elastic.co/blog/hot-warm-architecture-in-elasticsearch-5-x).

## Migrate indexes from hot nodes to warm nodes

1.  Create a config.yml file under the /usr/curator/ directory. Example:

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

    -   `hosts`: Set the value to the internal or public endpoint of the Elasticsearch cluster. The internal endpoint is used in this example.
    -   `http_auth`: Set the value to the username and password that are used to access the Elasticsearch cluster.
2.  Create an action.yml file under the /usr/curator/ directory. You can reference the following example to configure the file:

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

    In this example, indexes that are created on `hot` nodes 30 minutes ago and whose names start with `logstash-` are migrated to `warm` nodes. You can also configure the action.yml file as required.

3.  Run the following command to check whether the curator command runs normally:

    ```
    curator --config /usr/curator/config.yml /usr/curator/action.yml
    ```

    If the curator command runs normally, information similar to the following code is returned:

    ```
    2019-02-12 20:11:30,607 INFO      Preparing Action ID: 1, "allocation"
    2019-02-12 20:11:30,612 INFO      Trying Action ID: 1, "allocation": Apply shard allocation filtering rules to the specified indices
    2019-02-12 20:11:30,693 INFO      Updating index setting {'index.routing.allocation.require.box_type': 'warm'}
    2019-02-12 20:12:57,925 INFO      Health Check for all provided keys passed.
    2019-02-12 20:12:57,925 INFO      Action ID: 1, "allocation" completed.
    2019-02-12 20:12:57,925 INFO      Job completed.
    ```

4.  Run the following command to enable the curator command to run at 15-minute intervals:

    ```
    */15 * * * * curator --config /usr/curator/config.yml /usr/curator/action.yml
    ```


