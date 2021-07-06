# 什么是阿里云Elasticsearch

开源Elasticsearch是一个基于Lucene的实时分布式的搜索与分析引擎，是遵从Apache开源条款的一款开源产品，是当前主流的企业级搜索引擎。作为一款基于RESTful API的分布式服务，Elasticsearch可以快速地、近乎于准实时地存储、查询和分析超大数据集，通常被用来作为构建复杂查询特性和需求强大应用的基础引擎或技术。

阿里云Elasticsearch是基于[开源Elasticsearch](https://www.elastic.co/cn/elasticsearch/features)构建的全托管Elasticsearch云服务，在100%兼容开源功能的同时，支持开箱即用、按需付费。不仅提供云上开箱即用的Elasticsearch、Logstash、Kibana、Beats在内的Elastic Stack生态组件，还与Elastic官方合作提供免费X-Pack（白金版高级特性）商业插件，集成了安全、SQL、机器学习、告警、监控等高级特性，被广泛应用于实时日志分析处理、信息检索、以及数据的多维查询和统计分析等场景。

## 产品介绍

阿里云Elasticsearch致力于打造基于开源生态的、低成本、场景化的云上Elasticsearch解决方案，源于开源，又不止于开源。基于云上超强的计算和存储能力，以及在集群安全和运维领域长期积累的技术经验，阿里云Elasticsearch不仅支持集群一键部署、弹性伸缩、智能运维和各类内核引擎优化，还提供了迁移、容灾、备份和监控等全套解决方案。

您可以基于阿里云Elasticserch强大的分析检索能力，以及高安全、高性能、高可用的服务，简化集群部署管理工作、降低集群资源与运维成本、提升数据安全可靠性、打通上下游数据链路、优化读写性能效果等。基于这些优化，您可以快速构建日志分析、异常监控、企业搜索和大数据分析等各业务应用，聚焦于业务应用本身，实现业务价值。

## 产品组件

在阿里云Elastic Stack产品生态下，Elasticsearch作为实时分布式搜索和分析引擎，Kibana实现灵活的可视化分析，Beats从各个机器和系统采集数据，Logstash采集、转换、优化和输出数据。通过各个组件的结合，阿里云Elasticsearch可被广泛应用于实时日志处理、全文搜索和数据分析等领域。

-   [X-Pack](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/setup-xpack.html)

    X-Pack是Elasticsearch的一个商业版扩展包，包含安全Security、警告 Altering、监控Monitoring、图形Graph和报告Reporting、机器学习 MachineLearning等多种高级功能。创建阿里云Elasticsearch集群时，系统会默认将X-Pack作为插件集成在Kibana中，为您免费提供授权认证、角色权限管控、实时监控、可视化报表、机器学习等能力，实现更便捷的Elasticsearch运维管理和应用开发。

-   [Beats数据采集中心](/cn.zh-CN/Beats/采集ECS服务日志.md)

    Beats是轻量级的数据采集工具，集合了多种单一用途的数据采集器。它们从成百上千或成千上万台机器和系统向Logstash或Elasticsearch发送数据。

    阿里云Elasticsearch的Beats采集中心支持Filebeat、Metricbeat、Auditbeat和Heartbeat。支持在云服务器ECS（Elastic Compute Service）和容器服务Kubernetes版ACK（Container Service for Kubernetes）集群中一键部署采集器，可视化采集与配置日志文件、网络数据、容器指标等多种类型数据，并集中管理多个采集器。

-   [Logstash](/cn.zh-CN/Logstash/什么是阿里云Logstash.md)

    Logstash作为服务器端的数据处理管道，通过输入、过滤和输出插件，动态地从多个来源采集数据，并加工和转换任何类型的事件，最终将数据存储到所选择的位置。

    阿里云提供全托管的Logstash Service，100%兼容开源。支持一键部署、可视化配置和集中管理数据管道，提供多种插件实现与OSS、Maxcompute等云产品的连通。

-   [Kibana](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)

    Kibana是灵活的数据分析和可视化工具，支持多用户登录。在Kibana中，您可以搜索和查看Elasticsearch索引中的数据，并进行交互。创建阿里云Elasticsearch集群时，系统会自动部署独立的Kibana节点，您可以根据业务需求，灵活使用图表、表格、地图等，呈现多元化的数据分析报表和大盘。


## 相关服务

-   [AliES内核引擎及插件](/cn.zh-CN/AliES内核/内核版本发布记录.md)

    阿里云Elasticsearch在完全兼容开源Elasticsearch内核的所有特性基础上，在监控指标多样化、线程池、熔断策略优化、查询与写入性能优化等诸多方面，深度定制了AliES内核引擎。同时提供多种自研插件，提升集群稳定性、增强性能、优化成本并丰富监控运维功能。

-   [Eyou智能诊断系统](/cn.zh-CN/Elasticsearch/运维/智能运维/智能运维系统概述.md)

    阿里云Elasticsearch的智能运维系统EYou，提供集群、节点、索引等二十余个诊断项的健康检测功能。能够全面观测并记录集群的运行状况，自动归纳集群诊断结果。同时帮助您探测集群潜在风险，在集群异常状态下，快速提供关键信息和合理的优化建议，让集群运维更便捷。

-   [高级监控报警服务](/cn.zh-CN/高级监控报警/高级监控报警概述.md)

    高级监控报警服务是基于Elasticsearch开发的，具备采集、监控、报警、诊断、数据处理等多种能力的SAAS服务，为云上用户提供开箱即用的一站式监控报警解决方案。通过高级监控报警服务，您可以灵活配置Grafana监控大屏、自定义报警规则并使用稳定可靠的报警服务。帮助您更加方便地监控Elasticsearch集群各类指标信息，实时了解集群状况，及时定位并解决问题。


## 相关文档

-   产品优势
    -   [阿里云Elasticsearch与自建集群对比](/cn.zh-CN/产品简介/产品优势/阿里云Elasticsearch与自建集群对比.md)
    -   [高可用性](/cn.zh-CN/产品简介/产品优势/高可用性.md)
    -   [高安全性](/cn.zh-CN/产品简介/产品优势/高安全性.md)
    -   [高性能](/cn.zh-CN/产品简介/产品优势/高性能.md)
-   购买
    -   [购买阿里云Elasticsearch实例](/cn.zh-CN/Elasticsearch/实例管理/创建阿里云Elasticsearch实例.md)
    -   [购买阿里云Logstash实例]()
-   快速入门
    -   [Elasticsearch快速入门](/cn.zh-CN/Elasticsearch/快速入门.md)
    -   [Logstash快速入门](/cn.zh-CN/Logstash/快速入门.md)
    -   [Beats采集器快速入门](/cn.zh-CN/Beats/快速入门/入门概述.md)

