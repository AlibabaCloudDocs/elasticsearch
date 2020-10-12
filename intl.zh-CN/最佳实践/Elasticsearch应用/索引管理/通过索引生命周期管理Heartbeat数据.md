---
keyword: [es索引生命周期管理, es ILM管理]
---

# 通过索引生命周期管理Heartbeat数据

对于时间序列数据，会随着时间的积累越来越大，您可以通过索引生命周期管理ILM（Index Lifecycle Management）定期将数据滚动到新索引，防止因数据过大影响查询效率和成本。随着索引的老化和查询频率的降低，您可以将其转移到价格较低的磁盘上，并减少分片和副本的数量。本文介绍通过ILM管理Heartbeat数据的方法。

索引生命周期管理ILM是指Elasticsearch对索引进行设置、创建、打开、关闭、删除的全生命周期管理的过程。Elasticsearch（6.6.0及以上版本）提供了ILM功能，将索引生命周期分为4个阶段：hot、warm、cold、delete。其中hot阶段主要负责对索引进行滚动更新操作，warm、cold、delete阶段主要负责进一步处理滚动更新后的数据，详细说明如下。

|阶段|描述|
|--|--|
|hot|主要处理时序数据的实时写入。可根据索引的文档数、大小、时长决定是否调用[rollover API](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/indices-rollover-index.html)来滚动更新索引。|
|warm|索引不再写入，主要用来提供查询。|
|cold|索引不再更新，查询很少，查询速度会变慢。|
|delete|数据将被删除。|

本文使用的测试场景如下：

业务场景中存在大量的heartbeat-\*时序索引，并且每天单个索引大小都为4MB左右。当数据越来越多时，shard数量也会越来越多，导致集群负载增加。所以需要指定不同的滚动更新策略，在hot阶段滚动更新heartbeat-\*开头的历史监控索引，warm阶段对索引进行分片收缩及段合并，cold阶段将数据从hot节点迁移到warm（冷数据）节点，delete阶段定期删除索引数据。

## 操作流程

1.  [准备工作](#section_z64_ymo_p7z)

    创建阿里云Elasticsearch实例、开启自动创建索引功能、配置公网地址访问白名单。

2.  [步骤一：在Heartbeat下配置ILM](#section_i2s_5jq_een)

    在heartbeat.yml中开启阿里云Elasticsearch的生命周期管理功能，并配置其参数。配置完成并启动后，系统会自动在对应阿里云Elasticsearch实例中生成Heartbeat索引模板。

3.  [步骤二：创建ILM策略](#section_q4f_09f_b6h)

    通过ilm policy API创建生命周期管理策略，该策略用来定义索引滚动更新和归档的条件。

4.  [步骤三：为ILM策略关联索引模板](#section_xie_dsu_xvj)

    为上一步创建的生命周期管理策略关联Heartbeat索引模板。

5.  [步骤四：为索引关联ILM策略](#section_73o_c19_inw)

    为第一个Heartbeat索引关联生命周期管理策略，以便将该策略应用到整个Heartbeat索引模板覆盖的索引下。

6.  [步骤五：查看各阶段索引](#section_lew_8nm_29w)

    查看归档在各阶段（hot、warm、cold、delete）的索引。


## 准备工作

1.  创建阿里云Elasticsearch实例，并开启实例的自动创建索引功能。

    具体操作步骤请参见[创建阿里云Elasticsearch实例](/intl.zh-CN/快速入门/步骤一：创建实例/创建阿里云Elasticsearch实例.md)和[开启自动创建索引](/intl.zh-CN/快速入门/步骤二：配置实例（可选）.md)。

2.  配置阿里云Elasticsearch实例的公网地址访问白名单，将安装Heartbeat服务器的IP地址添加到白名单中。

    具体操作步骤请参见[配置ES公网或私网访问白名单](/intl.zh-CN/实例管理/安全配置/配置ES公网或私网访问白名单.md)。


## 步骤一：在Heartbeat下配置ILM

为了使Heartbeat与Elasticsearch的ILM无缝衔接，可在heartbeat.yml配置中定义Elasticsearch的ILM，详细配置请参见[Set up index lifecycle management](https://www.elastic.co/guide/en/beats/heartbeat/6.8/ilm.html)。

1.  下载[Heartbeat安装包](https://www.elastic.co/cn/downloads/past-releases#heartbeat)，并解压缩。

2.  编辑heartbeat.yml，分别定义`heartbeat.monitors`、`setup.template.settings`、`setup.kibana`、`output.elasticsearch`。

    本文使用的配置如下。

    ```
    heartbeat.monitors:
    - type: icmp
      schedule: '*/5 * * * * * *'
      hosts: ["47.111.xx.xx"]
    
    setup.template.settings:
      index.number_of_shards: 3
      index.codec: best_compression
      index.routing.allocation.require.box_type: "hot"
    
    setup.kibana:
    
      # Kibana Host
      # Scheme and port can be left out and will be set to the default (http and 5601)
      # In case you specify and additional path, the scheme is required: http://localhost:5601/path
      # IPv6 addresses should always be defined as: https://[2001:db8::1]:5601
      host: "https://es-cn-4591jumei00xxxxxx.kibana.elasticsearch.aliyuncs.com:5601"
    
    output.elasticsearch:
      # Array of hosts to connect to.
      hosts: ["es-cn-4591jumei00xxxxxx.elasticsearch.aliyuncs.com:9200"]
      ilm.enabled: true
      setup.template.overwrite: true
      ilm.rollover_alias: "heartbeat"
      ilm.pattern: "{now/d}-000001"
    
      # Enabled ilm (beta) to use index lifecycle management instead daily indices.
      #ilm.enabled: false
    
      # Optional protocol and basic auth credentials.
      #protocol: "https"
      username: "elastic"
      password: "<your_password>"
    ```

    部分参数说明如下，更多参数说明请参见[官方Heartbeat配置文档](https://www.elastic.co/guide/en/beats/heartbeat/6.7/configuration-heartbeat-options.html?spm=a2c4g.11186623.2.28.3a623279cDgZt0)。

    |参数|说明|
    |--|--|
    |`index.number_of_shards`|设置主分片数，默认是1。|
    |`index.routing.allocation.require.box_type`|设置将索引数据写入hot节点。|
    |`ilm.enabled`|设置为true，表示启用索引生命周期管理ILM。|
    |`setup.template.overwrite`|设置是否覆盖原索引模板。如果您已经将此版本的索引模板加载到Elasticsearch中，则必须将该参数设置为true，使用此版本的索引模板覆盖原索引模板。|
    |`ilm.rollover_alias`|设置滚动更新索引时，生成的索引的别名，默认是`heartbeat-\{beat.version\}`。|
    |`ilm.pattern`|设置滚动更新索引时，生成的索引的模式。支持[date math](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/indices-rollover-index.html#_using_date_math_with_the_rollover_api)，默认是\{now/d\}-000001。当触发索引滚动更新条件后，新的索引名称会在最后一位数字上加1。 例如第一次滚动更新产生的索引名称是heartbeat-2020.04.29-000001，当满足索引滚动更新条件后触发滚动，Elasticsearch会创建新的索引，名称为heartbeat-2020.04.29-000002。 |

    **说明：** 如果在加载索引模板后修改`ilm.rollover_alias`或`ilm.pattern`，则必须设置`setup.template.overwrite`为`true`，重写索引模板。

3.  启动Heartbeat服务。

    ```
    sudo ./heartbeat -e
    ```


## 步骤二：创建ILM策略

Elasticsearch支持通过API和Kibana控制台操作两种方式创建ILM策略。以下示例以API方式为例，介绍通过ilm policy API创建hearbeat-policy策略。

**说明：** Heartbeat支持通过`./heartbeat setup --ilm-policy`命令加载默认的策略并写入Elasticsearch，默认策略可通过`./heartbeat export ilm-policy`命令到stdout。您可以修改该默认策略，实现手动创建策略。

参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)，在Kibana中执行以下命令。

```
PUT /_ilm/policy/hearbeat-policy
{
  "policy": {
    "phases": {
      "hot": {
        "actions": {
          "rollover": {
            "max_size": "5mb",
            "max_age": "1d",
            "max_docs": 100
          }
        }
      },
      "warm": {
        "min_age": "60s",
        "actions": {
          "forcemerge": {
                "max_num_segments":1
              },
          "shrink": {
                "number_of_shards":1
              }
        }
      },
      "cold": {
        "min_age": "3m",
        "actions": {
          "allocate": {
            "require": {
              "box_type": "warm"
            }
          }
        }
      },
      "delete": {
        "min_age": "1h",
        "actions": {
          "delete": {}
        }
      }
    }
  }
}
```

|参数|说明|
|--|--|
|hot|该策略设置索引只要满足其中任一条件：数据写入达到5MB、使用超过1天、doc数超过100，就会触发索引滚动更新。此时系统将创建一个新索引，该索引将重新启动策略，而旧索引将在滚动更新后等待60秒进入warm阶段。 **说明：** 目前Elasticsearch支持在`rollover`中配置三种归档策略：max\_docs、max\_size、max\_age，满足其中任何一个条件都会触发索引归档操作。 |
|warm|索引进入warm阶段后，ILM会将索引收缩到1个分片，将索引强制合并为1个段。完成该操作后，索引将在3分钟（从滚动更新时算起）后进入cold阶段。|
|cold|索引进入cold阶段后，ILM将索引从hot节点移动到warm（冷数据）节点。完成操作后，索引将在1小时后进入delete阶段。|
|delete|索引进入delete阶段，将在1小时后被删除。|

## 步骤三：为ILM策略关联索引模板

启动Heartbeat后，系统会自动在对应的Elasticsearch中创建Heartbeat索引模板。您需要为[步骤二：创建ILM策略](#section_q4f_09f_b6h)中创建的自定义ILM策略关联该索引模板。

1.  登录目标阿里云Elasticsearch实例的Kibana控制台。

    具体步骤请参见[登录Kibana控制台](/intl.zh-CN/实例管理/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Management**。

3.  在**Elasticsearch**区域，单击**Index Lifecycle Policies**。

4.  在**Index lifecycle policies**列表中，单击**Actions** \> **Add policy to index template**。

    ![Add policy](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9112659951/p102589.png)

5.  在弹出的对话框中，选择**Index template**（heartbeat），并输入**Alias for rollover index**。

    ![Add policy](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0212659951/p102591.png)

6.  单击**Add policy**。


## 步骤四：为索引关联ILM策略

启动Heartbeat后，系统会自动在对应的Elasticsearch中创建索引。您需要为第一个索引关联对应的ILM策略，该ILM策略已经关联了索引模板（[步骤三：为ILM策略关联索引模板](#section_xie_dsu_xvj)）。

1.  在**Management**页面的**Elasticsearch**区域中，单击**Index Management**。

2.  在**Index management**列表中，找到目标索引，单击索引名称。

3.  在**Summary**页面，单击**Manage** \> **Remove lifecycle policy**，移除Heartbeat自带的默认策略。

    ![移除Heartbeat自带的默认策略](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0212659951/p102620.png)

4.  在弹出的对话框中，单击**Remove policy**。

5.  再单击**Manage** \> **Add lifecycle policy**。

6.  在弹出的对话框中，选择**Lifecycle policy**（上文的heartbeat-policy）和**Index rollover alias**（heartbeat），单击**Add policy**。

    ![Add policy](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0212659951/p102622.png)

    关联成功后，结果如下图。

    ![关联成功](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0212659951/p102623.png)


## 步骤五：查看各阶段索引

查看hot阶段的索引：在**Index management**页面，单击**Lifecycle phase** \> **Hot**。

![过滤Hot阶段索引](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0212659951/p102625.png)

您也可以使用同样的方式，查看其他阶段的索引。

## 附录：设置ILM策略周期

由于索引生命周期策略默认是10分钟检查一次符合策略的索引，因此在这10分钟内索引中的数据可能会超出指定的阈值。例如在[步骤二：创建ILM策略](#section_q4f_09f_b6h)时，设置max\_docs为100，但doc数量在超过100后才触发索引滚动更新，此时可通过修改`indices.lifecycle.poll_interval`参数来控制检查频率，使索引在阈值范围内滚动更新。

**说明：** 请慎重修改该参数值，避免时间间隔太短给节点增加不必要的负载，本测试中将其改成了`1m`。

```
PUT _cluster/settings
{
  "transient": {
    "indices.lifecycle.poll_interval":"1m"
  }
}
```

## 相关说明

-   索引必须定义模板和别名后，才可以设置生命周期管理策略。
-   您可以通过两种方式给索引添加生命周期管理策略。

    |添加周期策略的方式|说明|
    |---------|--|
    |对索引模板添加生命周期管理策略|将策略应用到整个别名覆盖的索引下。|
    |对单个索引添加生命周期管理策略|只能覆盖当前索引，新滚动的索引不再受策略影响。|

-   如果在滚动更新索引时修改了生命周期管理策略，新策略将在下一次滚动更新时生效。

