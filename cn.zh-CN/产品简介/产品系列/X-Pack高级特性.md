# X-Pack高级特性

本文将从特性简介、购买指引、功能说明和版本功能详细对比等方面为您介绍X-Pack高级特性，方便您快速了解X-Pack高级特性。

## 特性简介

X-Pack高级特性，是Elasticsearch基于原X-Pack商业版插件开发的官方商业版特性，包含了安全、SQL、机器学习、告警、监控等多个高级特性，从应用开发和运维管理等方面增强了Elasticsearch的服务能力。

阿里云Elasticsearch目前已经提供了包含X-Pack高级特性的版本，您可以在创建集群时选择购买，下文将介绍此特性的详细信息。

## 购买指引

您可以登录[阿里云Elasticsearch控制台](https://elasticsearch-cn-hangzhou.console.aliyun.com/#/home)，在Elasticsearch实例页面单击创建进行购买。当前支持X-Pack高级特性的版本有通用商业版和日志增强版，如下图所示：

![ES_fig_X-Pack_01](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5847767161/p260403.png)

对应版本的信息如下表所示：

|对比项|通用商业版|日志增强版|
|---|-----|-----|
|是否包含X-Pack|是|是|
|X-Pack功能是否完整|是|是|

**说明：**

阿里云Elasticsearch通用商业版和日志增强版实例除具备X-Pack高级特性外，还具备运维管理、安全、插件、高可用等功能，具体介绍请参见[通用商业版实例介绍](https://help.aliyun.com/document_detail/188205.html?spm=a2c4g.11186623.6.554.4e9d61135QDz1W)和[增强版实例介绍](https://help.aliyun.com/document_detail/143099.html?spm=a2c4g.11186623.6.555.7c20709eiYGvNm)。

## 功能说明

本文仅针对部分常用的高级特性进行说明，完整的高级特性说明，请参见[Elastic Stack 订阅](https://www.elastic.co/cn/subscriptions)和[API 文档](https://www.elastic.co/guide/en/elasticsearch/reference/6.4/xpack-api.html)。

**说明：** 官方Elastic Stack在不同的版本（例如：免费开源、黄金级和PLATI­NUM（白金版））提供的高级特性存在一定差异，阿里云Elasticsearch订阅的是PLATI­NUM（白金版）。

|常用功能项|功能项说明|
|-----|-----|
|安全|对索引和字段实现分权管理，严控访问权限，提升数据安全性。|
|机器学习|数据实时监控，具备自预警和上报告警功能。|
|监控|多维度（如集群，节点，索引等）实时运行监控，提升开发效率，降低运维成本。|
|SQL|-   通过传统SQL数据库，实现对Elasticsearch数据的全文本检索和数据统计分析功能。
-   支持CLI、REST等接入方式（PLATI­NUM（白金版）的SQL插件还支持JDBC连接）。
-   同原有业务系统无缝对接，降低了新技术的学习成本。

**说明：** 开源版集成了其他的SQL 插件，详细信息请参见[elasticsearch-sql](https://github.com/NLPchina/elasticsearch-sql)。 |

## 版本功能详细对比

本文主要针对开源版和PLATI­NUM（白金版）的X-Pack部分重点功能进行对比说明，方便您了解不同版本中功能的区别。因为Elasticsearch正处于快速发展阶段，不同版本对各功能的支持情况也在不断更新。如需获取官方Elasticsearch最新的功能对比，请参见[Elastic Stack 订阅](https://www.elastic.co/cn/subscriptions)。

版本功能详细对比请参见下表：

**说明：** 下表中√、○、×分别用于表示对应特性的功能完整度，其中：

-   √：表示包含全部功能。
-   ○：表示包含部分功能。
-   ×：表示不包含的功能。

|模块|特性|免费开源版|PLATI­NUM（白金版）|
|--|--|-----|--------------|
|ElasticSearch|可扩展性和弹性|○|√|
|查询和分析|○|√|
|堆栈管理|○|√|
|Security|×|√|
|机器学习|×|√|
|Watcher|×|√|
|kibana|探索和可视化|○|√|
|堆栈管理|○|√|
|堆栈检测|×|√|
|kibana告警|×|√|
|Security|×|√|
|机器学习|×|√|
|Beats|数据收集|○|√|
|数据传输|○|√|
|监测和管理|×|√|
|模块|○|√|
|Logstash|数据收集|√|√|
|数据扩充|√|√|
|数据传输|√|√|
|模块|○|√|
|监测和管理|×|√|
|Elastic APM|APM服务器|√|√|
|APM代理|√|√|
|kibana APM仪表盘|√|√|
|APM UI|×|√|
|分布式追踪|×|√|
|Machine Learning 整合|×|√|
|Elastic日志|日志采集器（Filebeat）|√|√|
|常用数据源仪表板|√|√|
|Logs UI|×|√|
|Elastic基础设施|指标采集器|√|√|
|常用数据源仪表板|√|√|
|Infrastructure UI|×|√|
|Elastic 运行状态监控|运行状态监测（Heartbeat）|√|√|
|Kibana里的运行状态仪表板|√|√|
|运行状态监测UI|×|√|

## Elasticsearch部分功能详细说明

Elasticsearch部分功能详细说明请参见下表：

|功能大类|功能二级分类|功能三级分类|
|----|------|------|
|管理和运行|可扩展和弹性|聚类和高可用性|
|自动节点恢复|
|自动数据再平衡|
|水平可扩展性|
|机架感知|
|跨集群复制|
|跨数据中心复制|
|MONITORING|全堆栈检测|
|多堆栈检测|
|可配置保留政策|
|堆栈发生问题时自动告警|
|管理|索引生命周期管理|
|数据层|
|冻结索引|
|快照和还原|
|可搜索快照|
|纯源快照|
|快照生命周期管理|
|数据汇总|
|数据流|
|CLI工具|
|升级助手UI|
|升级助手API|
|用户和角色管理|
|Transforms|
|ALERTING|高可用性、可扩展警报|
|通知|
|Alerting UI|
|STACK安全性|安全设置|
|加密通信|
|数据静态加密支持|
|基于角色的访问控制|
|字段级和文档级安全性|
|审计日志|
|IP筛选|
|Security Realm|
|单点登录SSO|
|第三方安全性集成|
|客户端|RESTAPI|
|语言客户端|
|Console|
|DSL|
|SQL|
|时间查询语言EQL|
|JDBC客户端|
|ODBC客户端|
|采集和扩充|数据源|操作系统|
|网络服务器和代理|
|数据存储库和队列|
|云服务|
|容器|
|网络数据|
|安全数据|
|运行状态数据|
|文件导入|
|数据扩充|处理器|
|分析器|
|分词器|
|筛选器|
|语言分析器|
|Grok|
|字段转化|
|外部查询|
|enrich|
|Geo enrich|
|模块集成|客户端、API|
|Beats|
|社区采集agent|
|Logstash|
|ES-Hadoop|
|插件扩展|
|数据存储|灵活性|数据类型|
|全文本搜索|
|文档数据库|
|时序/分析|
|地理空间|
|SECURITY|数据静态加密支持|
|字段级安全性|
|管理|集群式索引|
|数据快照和还原|
|汇总索引|
|搜索和分析|全文本搜索|倒排索引|
|跨集群搜索|
|相关性评分|
|查询DSL|
|非同步搜索|
|Highlighter|
|自动补全|
|拼写检查和更正|
|提示|
|Percolator|
|查询优化器|
|基于许可的搜索结果|
|取消查询|
|分析|聚合|
|Graph搜索|
|阈值告警|
|机器学习|推理|
|时序预测|
|时序异常监测|
|异常情况报警|
|APM|APM Server|
|APM 代理|
|APM 应用|
|分布式跟踪|
|告警|
|服务地图|
|可视化|仪表板|
|Canvas|
|Kibana Lens|
|时序可视化生成器|
|图表分析|
|地理空间分析|
|容器监测|
|Kibana插件|
|数据导入教程|
|MAPS|地图图层|
|定制区域地图|
|GeoJson上传|
|ELASTICLOGS|日志采集器|
|Logs仪表盘|
|日志速率异常检测|
|ELASTICMETRICS|指标采集器|
|仪表板|
|告警|
|UPTIME|监控|
|仪表板|
|告警|
|证书检测|
|合成检测|
|安全分析|Common Schema|
|安全分析|
|时间线事件|
|案例管理|
|异常检测|

