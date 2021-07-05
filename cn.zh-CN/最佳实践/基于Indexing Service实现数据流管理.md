# 基于Indexing Service实现数据流管理

通过使用阿里云Elasticsearch 7.10日志增强版Indexing Service系列，可以为您实现云托管写入加速和按流量付费（即您无需按集群峰值写入吞吐预留资源），能够极低成本实现海量时序日志分析。本文为您介绍如何基于Indexing Service系列实现数据流管理以及日志场景分析。

在复杂业务场景下，海量服务器、物理机、Docker容器、移动设备和IoT传感器等设备中往往存在着结构分散、种类多样且规模庞大的各类指标和日志数据，而除了底层系统的各类指标和日志数据外，往往还存在着规模庞大的业务数据，例如用户行为、行车轨迹等。当面对海量时序数据和日志数据写入出现性能瓶颈时，您可以根据业务需求选择使用阿里云Elasticsearch 7.10日志增强版[Indexing Service系列](/cn.zh-CN/产品简介/产品系列/日志增强版实例介绍/Indexing Service系列介绍.md)，此功能基于读写分离架构以及写入按量付费的Serverless模式，实现了Elasticsearch集群的云端写入托管和降本提效的目标。

在阿里云Elasticsearch 7.10日志增强版Indexing Service系列中，推荐使用[数据流管理](/cn.zh-CN/Elasticsearch/索引管理中心/数据流管理.md)，可以帮您实现跨多索引存储仅追加时间序列数据，为请求提供唯一的命名资源；并且您可以根据关联的索引模板和Rollover策略实现自动取消托管，从而达到云端托管数据的自动清理和成本优化。数据流管理非常适用于日志、事件、指标和其他连续生成数据的场景。除此之外，您还可以通过使用索引生命周期管理（[ILM](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-lifecycle-management.html?spm=a2c4g.11186623.2.18.11224ebdEseAr7)）定期管理后备索引，帮助您降低成本及开销。

Elasticsearch集群中既可以存在数据流（[Data Stream](https://www.elastic.co/guide/en/elasticsearch/reference/current/data-streams.html?spm=a2c4g.11186623.2.14.7ea74ebdshmL9D#data-streams)），也可以存在独立索引（Index）对象。除系统索引不托管外，其他索引均默认开启云端托管功能。独立索引支持增、删、改、查操作，操作前需要您手动取消云端托管。为了帮助您更好的使用数据流管理云端托管索引，阿里云Elasticsearch控制台分别提供了[数据流管理](/cn.zh-CN/Elasticsearch/索引管理中心/数据流管理.md)、[索引管理](/cn.zh-CN/Elasticsearch/索引管理中心/索引管理.md)和[索引模板管理](/cn.zh-CN/Elasticsearch/索引管理中心/索引模板管理.md)功能模块，通过白屏化的方式为您实现数据流一站式管理。

## 实践场景

本文通过将采集到的nginx服务日志数据，写入到阿里云Elasticsearch 7.10日志增强版Indexing Service系列实例中，通过数据流管理和索引生命周期管理，实现日志数据的分析和检索。

## 前提条件

-   因为数据流写入依赖时间字段`@timestamp`，所以请确保写入数据中存在`@timestamp`字段的数据，否则数据流写入过程中会报错。如果源数据中没有`@timestamp`字段数据，您可以使用[ingest pipeline](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/ingest.html#access-ingest-metadata)指定`_ingest.timestamp`，获取元数据值，从而引入`@timestamp`字段数据。
-   Indexing Service提供了写入Serverless保护机制，因此使用前请参见[使用限制](/cn.zh-CN/产品简介/产品系列/日志增强版实例介绍/Indexing Service系列介绍.mdsection_yw5_2ur_j6q)，提前优化配置，以避免使用过程中出现不合规情况。

## 操作流程

1.  [步骤一：创建实例](#section_7bx_jgb_tfi)

    创建一个阿里云Elasticsearch 7.10日志增强版Indexing Service系列的实例。

2.  [步骤二：创建索引模板](#section_aca_65k_2gs)

    在使用数据流之前，需要创建索引模板，通过模板对数据流后备索引进行结构配置。

3.  [步骤三：创建数据流](#section_cm3_o7e_u6r)

    创建数据流并写入数据。

4.  [步骤四：托管索引管理](#section_p0l_gty_ksf)

    对数据流或者独立索引进行云端托管管理。

5.  [步骤五：查看集群信息](#section_114_nja_jyc)

    在节点可视化页面查看集群当天写入的总流量以及写入托管总数量。

6.  [步骤六：日志分析](#section_pz2_nge_ed5)

    在Kibana控制台中，查看基于Indexing Service实现的数据流管理的实时日志流和实时数据指标。


## 步骤一：创建实例

1.  登录[阿里云官网](https://www.aliyun.com/)。

2.  在**产品**下拉列表中，搜索**Elasticsearch**。

3.  单击**新购ELK**，选择**日志增强版**。

    **说明：** 如果您是首次购买阿里云Elasticsearch集群，请单击**0元开通ELK**。

    ![ES购买页](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6962545261/p291202.png)

    |选项|默认值|说明|
    |--|---|--|
    |付费模式|无|支持以下两种方式：    -   包年包月
    -   按量付费 |
    |选择服务|无|支持以下两种服务版本：    -   通用商业版
    -   日志增强版
**说明：** 仅阿里云Elasticsearch日志增强版7.10支持Indexing Service系列。本文以**7.10（推荐）**版本为例。 |
    |系列|Indexing Service|阿里云Elasticsearch 7.10日志增强版Indexing Service提供云端托管能力，实现了低成本下，提高数据写入速度。|
    |场景初始化配置|日志场景|阿里云Elasticsearch 7.10日志增强版默认应用**日志场景**模板，使集群配置适配于日志场景。|

4.  单击**下一步：集群配置**，配置**地域和可用区**、**可用区数量**和**实例规格**等信息。

    ![基础配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6962545261/p291204.png)

    |资源|说明|
    |--|--|
    |地域和可用区|日志增强版Indexing service系列支持的地域和可用区。**说明：** 可用区选择建议尽可能和云端其他业务保持在同一区域，提高业务的集中化管理。 |
    |可用区数量|    -   **单可用区**：普通部署模式，适用于非关键任务型工作（默认）。
    -   **两个可用区**：跨可用区容灾部署模式，适用于生产型工作。
    -   **三个可用区**：高可用部署模式，适用于具有更高可用性要求的生产型工作。 |
    |写入Serverless资源|在云托管中，ES集群写入模块按实际写入流量和托管存储空间按量付费，在集群创建后按每小时计费。|
    |查询集群资源|当您配置集群资源时，可按查询业务要求进行冷、热节点资源配置。    -   写入云端托管场景下，当您查询集群无写入计算压力时，推荐使用冷数据节点配置，降低资源成本。
    -   如果您对日志数据查询时延有较高要求，可以对冷、热数据节点进行规划，选择规格合适的热数据节点。 |

5.  单击**下一步：网络与配置**，配置**专有网络**、**实例名称**和**登录密码**等信息。

    ![网络系统配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6962545261/p291205.png)

    **说明：** 购买前，需提前在待购买的可用区下配置好VPC网络环境，建议和业务所在环境处于同一网络下。

6.  单击**下一步：确认订单**。

7.  选中**服务协议**，单击**立即购买**。

    **说明：**

    -   实例创建后，需要一段时间才能生效。时间长短与您的集群规格、数据结构和大小等相关，一般在小时级别。
    -   当实例的信息没有及时更新时，例如刚创建完成的实例状态显示失败，可在**基本信息**页面，单击**刷新**，手动刷新页面中的状态信息。

## 步骤二：创建索引模板

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  进入目标实例。

    1.  在顶部菜单栏处，选择资源组和地域。

    2.  在左侧导航栏单击**Elasticsearch实例**，然后在**Elasticsearch实例**中单击目标实例ID。

3.  在左侧导航栏，选择**配置与管理** \> **索引管理中心**。

4.  单击**索引模板管理**页签。

5.  单击**创建索引模板**。

6.  在**创建索引模板**面板，配置**索引生命周期策略配置**。

    1.  选择**索引生命周期策略**。

    2.  输入**策略名称**，单击**保存并下一步**。

        ![创建索引模板](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6962545261/p291207.png)

        |选项|说明|
        |--|--|
        |新建索引生命周期策略|创建新的索引生命周期策略。|
        |选择已有索引生命周期策略|集群中存在服务业务逻辑策略，点击下拉框选择即可。|
        |跳过此步|对数据流后备索引不进行生命周期策略管理。|

    本步骤使用的命令示例值如下：

    ```
    PUT /_ilm/policy/nginx_policy
    {
      "policy": {
        "phases": {
          "hot": {
            "actions": {
              "rollover": {
                "max_size": "30GB",
                "max_age": "1d",
                "max_docs": 1000000
              },
              "set_priority" : {
                "priority": 1000
              }
            }
          },
          "delete": {
            "min_age": "7d",
            "actions": {
              "delete": {}
            }
          }
        }
      }
    }
    ```

    **说明：** 通过以上示例值新建的索引生命周期策略，表示当托管索引写入文件数超过1000000或索引大小达到30 GB或索引从创建开始满1天时，将触发滚动更新，生成新的后备索引，原索引保留7天后将自动删除。

7.  输入**模板名称**和**索引模式**，配置索引模版信息。`*`表示必填项。

    ![索引模板配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7962545261/p291208.png)

    |名称|说明|
    |--|--|
    |模板名称|定义的模板名称。|
    |索引模式|定义索引模式，使用通配符（\*）表达式匹配数据流及索引名称，不允许使用空格和字符 `\`、`/`、 `?`、`"`、`<`、 `>`和 `|`。|
    |创建数据流|开启数据流模式。如果未开启，索引模式无法生成数据流。详细信息，请参见[data stream](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/set-up-a-data-stream.html#create-data-stream)。|
    |优先级|定义模板优先级，数值越大，优先级越高。|
    |索引生命周期策略|只能引用一个索引生命周期策略。|
    |内容模板配置|配置索引[Settings](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/index-modules.html#index-modules-settings)、[Mappings](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/mapping.html)和[Aliases](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-template.html#_index_template_with_index_aliases)。**说明：**

    -   数据流后备索引文档必须包含一个@timestamp映射为[date](https://www.elastic.co/guide/en/elasticsearch/reference/current/date.html)或者[date\_nanos](https://www.elastic.co/guide/en/elasticsearch/reference/current/date_nanos.html)类型的字段，建议在索引模板中为@timestamp字段指定映射，如果不指定，则Elasticsearch会默认@timestamp映射为date类型。
    -   配置格式严格按照Elastic官方配置。 |

    本步骤使用的命令示例值如下：

    ```
    PUT /_index_template/nginx_telplate
    {
      "index_patterns": [ "nginx-*" ],
      "data_stream": { },
      "template": {
        "settings": {
          "index.number_of_replicas": "1",
            "index.number_of_shards": "6",
          "index.refresh_interval": "5s",
          "index.lifecycle.name": "nginx_policy",
          "index.apack.cube.following_index": true
        }
      },
      "priority": 100
    }
    ```

    **说明：**

    -   通过命令创建模板时，务必将`index.apack.cube.following_index`设置为true。
    -   云端托管集群上`index.refresh_interval`参数已默认配置最优，手动配置不生效。如果需要通过手动配置`index.refresh_interval`生效，需要先取消云托管功能。
8.  单击**确定**，索引模板管理页签下将显示对应的模板。

    ![显示对应的模板](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7962545261/p291209.png)


## 步骤三：创建数据流

等待索引模板创建成功后，进行以下操作。

1.  在左侧导航栏，选择**配置与管理** \> **索引管理中心**。

2.  单击**数据流管理**页签。

3.  单击**创建数据流**。

4.  在**创建数据流**面板，单击**预览已有索引模板**，根据对应的索引模板，输入可匹配索引模板的数据流名称。

    ![创建数据流](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7962545261/p291210.png)

    本步骤使用的命令示例值如下：

    ```
    PUT /_data_stream/nginx-log
    ```

    **说明：**

    -   创建数据流之前必须存在数据流可匹配的索引模板，该模板包含用于配置数据流的后备索引映射及设置。
    -   数据流命名不支持匹配符星号（\*），支持短划线（-）结尾。
5.  单击**确定**，自动生成数据流及后备索引。

    ![查看生成的数据流](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7962545261/p291211.png)

    每个数据流创建成功后，都会自动生成一个统一格式的后备索引，格式如下：

    ```
    .ds-<data-stream>-<yyyy.MM.dd>-<generation>
    ```

    |格式|说明|
    |--|--|
    |.ds|隐藏索引名统一标识，数据流生成的后备索引名，默认均以`.ds`开头。|
    |<data-stream\>|数据流名称。|
    |<yyyy.MM.dd\>|后备索引创建日期。|
    |<generation\>|每个数据流都会生成一个六位数，默认从000001开始的累积整数值，generation值更大的后备索引包含更多新数据。|

6.  写入数据。详细指导请参见[最佳实践](/cn.zh-CN/最佳实践/Elasticsearch应用/日志同步分析/使用Filebeat+Kafka+Logstash+Elasticsearch构建日志分析系统.md)。

    数据写入过程中，必须带`@timestamp`字段，否则写入失败。本场景采用filebeat+kafka+logstash架构将日志采集写入到Elasticsearch实例中，采集过程中会自动生成`@timestamp`字段。命令示例如下：

    ```
    POST /nginx-log/_doc/
    {
      "@timestamp": "2099-03-07T11:04:05.000Z",
      "user": {
        "id": "vlb44hny"
      },
      "message": "Login attempt failed"
    }
    ```


## 步骤四：托管索引管理

1.  在左侧导航栏，选择**配置与管理** \> **索引管理中心**。

2.  单击**索引管理**页签，查看处于云托管状态的索引。

    ![查看处于云托管中的索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0659590261/p273689.png)

    |功能|说明|
    |--|--|
    |仅查看托管中的索引|    -   选中此功能（建议）：仅显示托管中的索引，帮助您快速获取处于托管的数据。
    -   取消选中此功能：展示集群所有索引（不包括系统索引）。 |
    |云端托管索引总大小|当前时刻，正处于云端写入托管中的索引总大小。 **说明：** 云端托管索引总大小为实时变化数值，不是历史索引总大小。 |
    |索引个数|当前时刻，正处于云端写入托管中的索引总个数。 该数值为当前系统中的实时数值。**说明：** 索引个数为实时变化数值，不是历史索引总个数。 |
    |写入托管状态|    -   开启：该索引的云端写入托管处于开启状态。默认开启。
    -   关闭：取消该索引的云端写入托管。支持手动关闭，关闭后不支持开启。
**说明：**

    -   手动关闭某一索引的云端写入托管，数据将直接写入用户集群中。请在关闭前确认该索引是否持续有数据写入，以及用户集群负载情况，否则可能出现用户集群负载较高风险。
    -   业务上建议使用[数据流（Data Stream）](https://www.elastic.co/guide/en/elasticsearch/reference/current/data-streams.html)和索引生命周期管理（ILM）滚动策略，实现云端托管空间最优化。
    -   在独立索引的云端写入托管过程中，索引数据会全量存储在云托管服务Indexing Service中，将会增加云托管费用。请根据业务使用场景（如索引是否仍有数据写入）评估是否需要手动关闭该索引的写入托管。 |

    **说明：** 由于数据流nginx-log配置了索引滚动策略，所以在云托管服务上，每次仅保存最新生成的后备索引（本场景中的**.ds-nginx-log-2021.04.26-000004**），旧的后备索引会自动从云托管上关闭。

3.  单击**开启/关闭**按钮，在**取消托管**弹框中，单击**确定**，即取消托管。

    ![开启或关闭云托管状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9729360261/p272239.png)

    **说明：**

    -   取消云托管后，无法再次开启云端Indexing service写入托管功能。
    -   Elasticsearch集群中既可以存在数据流（Data Stream），又可以存在独立索引（Index）对象，除系统索引不托管外，其他索引均默认开启托管功能。
    -   可通过[Indexing Service API](/cn.zh-CN/Elasticsearch/索引管理中心/索引管理.mdsection_bhc_7fi_jaj)获取更多Indexing Service托管集群信息。
    对于独立索引或未设置滚动策略的索引，将一直在云托管服务保存，需要手动关闭。例如手动关闭`test`索引托管，写入托管状态将处于关闭中。

    ![关闭独立索引](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1659590261/p273694.png)

    本步骤使用的命令示例值如下：

    ```
    POST /nyc_taxis/_cube/unfollow
    ```


## 步骤五：查看集群信息

1.  [进入节点可视化页面](/cn.zh-CN/Elasticsearch/实例管理/查看集群状态和节点信息.md)，查看写入Indexing Service实时写入流量和数据量信息。

2.  在**Indexing Service**页面，单击**当天写入总流量**，即可查看**每小时平均写入吞吐量**的曲线图。

    ![每小时写入平均吞吐量](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8882545261/p291217.png)

    **说明：** Indexing Service写入总流量监控为非实时整点展示的静态趋势监控图，监控数据展示延时最长为1小时，例如在14:00~14:59间写入的总流量，需要等到15:10分后，在监控页面14:00可以获取到。

3.  单击**查看监控详情**，将跳转至Grafana监控展示更详细的监控数据。

    **说明：** Grafana的登录名密码请从[高级监控报警](https://elasticsearch.console.aliyun.com/cn-beijing/esmonitor/alarm-group)获取。

4.  在**Indexing Service**页面，单击**写入托管总数据量**，即可查看**当天写入托管总数据量**。

    ![当天写入托管总数据量](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8882545261/p291219.png)

    **说明：** Indexing Service写入总流量监控为非实时整点展示的静态趋势监控图，监控数据展示延时最长为1小时，例如在14:00~14:59间写入的总流量，需要等到15:10分后，在监控页面14:00可以获取到。


## 步骤六：日志分析

1.  登录对应阿里云Elasticsearch实例的Kibana控制台。

    具体操作，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  创建索引模板。

    1.  单击左上角![进入kibana](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0829360261/p272290.png)。

    2.  在左侧导航栏，选择**Management** \> **Stack Management**。

    3.  在**Stack Management**界面的**Kibana**区域，单击**index Patterns**。

3.  单击**Create index pattern**，进入到**Create index pattern**界面。

    ![创建索引模板](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8846980261/p273416.png)

    **说明：** index pattern匹配不仅可以指定为数据流，也可以指定为后备索引。

4.  设置**Settings**。

    1.  单击左上角![进入kibana](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0829360261/p272290.png)。

    2.  在左侧导航栏，选择**observability** \> **Logs**。

    3.  在**Logs**界面，单击**Settings**。

    4.  输入**log indices**，此处以本文创建的数据流名称**nginx-log**为例，其他字段的默认配置符合数据流数据要求，可以不做修改。

        ![设置settings](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8846980261/p273441.png)

    5.  在右下角，单击**apply**。

5.  获取实时日志流数据。

    1.  单击**Stream**页签。

        ![stream](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9567980261/p273450.png)

    2.  单击**Stream live**。

        ![stream live](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9567980261/p273452.png)

    获取的实时数据流如下所示：

    ![实时数据流](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9567980261/p273465.png)

6.  获取实时数据指标。

    1.  单击左上角![进入kibana](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0829360261/p272290.png)。

    2.  在左侧导航栏，选择**Kibana** \> **Discover**。

        获取的实时数据指标如下所示：

        ![实时数据流指标](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0667980261/p273469.png)


更多Kibana日志分析功能请参考[kibana Guide](https://www.elastic.co/guide/en/kibana/7.4/index.html)。

