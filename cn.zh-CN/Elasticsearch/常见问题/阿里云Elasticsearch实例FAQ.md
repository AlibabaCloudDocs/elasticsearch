# 阿里云Elasticsearch实例FAQ

本文汇总了使用阿里云Elasticsearch（简称ES）实例时的常见问题。

-   购买或退订实例问题
    -   [购买ES实例时选错配置，如何处理？](#section_pqi_rb3_djh)
    -   [ES购买页的版本具体对应的是哪个版本？](#section_ecn_32x_4n1)
    -   [购买ES实例时，专有网络为空，如何处理？](#section_8k3_wpr_32f)
    -   [购买ES实例时，已拥有对应的专有网络，但无法选择虚拟交换机或虚拟交换机为空，如何解决？](#section_3ua_8iw_s0v)
    -   [已购买的实例退订后重新购买，实例的访问地址会变吗？](#section_3el_7d8_y1n)
    -   [如何释放ES实例？](#section_p92_cav_ju9)
    -   [ES实例停止服务后多久被释放？](#buyb)
    -   [我可以购买单机版的ES实例吗？](#buyc)
    -   [购买实例时，资源已经售罄怎么办？](#buyd)
-   产品功能咨询问题
    -   [ES支持版本升级或降级吗？](#section_l6j_4pw_luc)
    -   [ES支持通过SSH登录集群修改配置吗？](#section_66y_1yu_14l)
    -   [6.7版本的Logstash和6.3版本的ES能够兼容吗？](#section_jjo_687_oxh)
    -   [Quick BI支持ES数据源吗？](#section_wzs_gwf_dt0)
    -   [ES支持评分插件吗？](#section_qwb_pxa_t7i)
    -   [ES支持LDAP功能吗？](#section_0b9_ew5_2bn)
    -   [ES有Java SDK吗？](#aa)
    -   [ES实例的内核版本在哪里查看？](#section_6ee_70y_8kh)
    -   [阿里云ES支持本地部署吗？](#section_u1i_4c6_pvd)
-   重启实例问题
    -   [重启ES实例或节点需要多久？](#section_a97_rci_u7u)
    -   [打开或关闭ES实例的公网访问时，会触发实例重启吗？](#section_lsf_6pk_nd8)
-   数据查询或写入问题

    [使用ES时，一部分节点的CPU和负载正常，另一部分处于空闲状态，如何处理？](#section_zto_kls_xvc)

-   集群配置与变更问题
    -   [使用ES前，如何合理规划集群的资源和规格以及shard的大小和数量？](#section_yus_2re_7ho)
    -   [如何查看ES实例的配置参数？](#section_1hd_bc2_sjo)
    -   [变更集群配置会影响ES服务吗？](#section_mri_z28_4kc)
    -   [ES支持变更云盘类型吗？](#section_9ht_ho3_e90)
    -   [ES支持将其他类型的节点变为冷数据节点吗？](#section_cx4_sm8_2jp)
    -   [升级了实例规格后，可以降低配置吗，如何操作？](#section_xp7_7ct_mjg)
    -   [业务量临时突增，如何变更集群配置，来保证业务正常进行？](#section_rdh_36n_v9k)
    -   [升配集群时，提示UpgradeVersionMustFromConsole如何处理？](#ymlb)
    -   [升级ES版本需要多长时间？](#ymlc)
    -   [升级ES版本会影响集群服务吗？](#ymld)
    -   [ES支持修改JVM参数吗？](#ymla)
    -   [是否可以在集群的YML文件配置中，调整http.max\_content\_length和discovery.zen.ping\_timeout值？](#section_e54_c8p_2ei)
    -   [我可以切换ES实例的VPC吗？](#section_sse_1yg_dng)
-   插件、分词、同义词问题
    -   [使用IK分词器时，如何自定义扩展分词词典内容？](#section_7du_5a2_uzc)
    -   [使用IK分词插件时，提示ik startOffset报错，如何处理？](#section_msh_ad9_he4)
    -   [本地IK词库文件丢失，可以在集群管理页面找回吗？](#section_swe_lom_z23)
    -   [更新IK分词词库后，如何使新的词库对之前的数据生效？](#section_ewc_qn2_fey)
    -   [FullGC有标准值吗？](#section_4k3_xz1_0o2)
    -   [不使用的插件可以卸载吗？](#ra)
    -   [ES的IK分词插件和开源版本的IK插件的词库一致吗？](#rb)
    -   [自定义插件可以访问外部网络吗，例如读取Github上的词库文件？](#rc)
    -   [自定义插件支持热更新功能吗？](#rd)
    -   [analysis-aliws分词是如何配置的，文件的格式是什么样的？](#re)
    -   [ES同义词、IK分词、AliNLP分词有哪些区别？](#section_3ui_quz_3oa)
-   日志问题
    -   [ES支持设置.security日志的保存时间吗？](#section_fhh_6jq_ppy)
    -   [ES只能看到7天内的日志，如何查看更多的历史日志？](#section_6qv_ism_v2t)
    -   [查看不到ES的查询和更新日志，如何处理？](#section_erh_hy8_del)
    -   [如何配置和查看ES实例的慢日志？](#rza)
    -   [如何通过程序定期拉取ES的慢日志？](#rzb)
-   数据备份与恢复问题
    -   [ES实例的快照能恢复到其他版本的实例中吗？](#section_czy_aqx_qu2)
    -   [备份ES数据时，提示集群状态不健康，如何处理？](#section_8bi_k19_zdi)
    -   [开启了自动备份，但是没有设置过OSS，是不是没有备份成功？](#section_pj5_ckd_rdv)
    -   [通过快照迁移（恢复）数据时，目标端显示分片异常。通过分片恢复指令POST /\_cluster/reroute?retry\_failed=true依旧无法成功，且对应索引显示异常，如何处理？](#section_24w_qo5_308)
    -   [ES中的数据是否可以导出到本地？](#backupa)
-   集群监控报警问题
    -   [如何使用X-Pack Watcher的邮件提醒功能？](#section_268_v23_5fr)
    -   [出现GC内存无法分配的报警，如何处理？](#section_m0x_9og_rkr)
    -   [ES是否支持Grafana监控？](#Grafana)
-   访问集群问题
    -   [如何使用客户端连接阿里云ES集群，与开源ES有什么区别？](#section_id1_eed_y9x)
    -   [通过客户端访问ES实例时，可以关闭Basic Auth（安全认证）吗？](#section_cwg_j34_x6v)
    -   [阿里云ECS和ES的VPC相同，但可用区不同，那ECS可以通过内网访问ES吗？](#visita)
    -   [如何通过外网连接ES实例？](#visitb)

## 购买ES实例时选错配置，如何处理？

当您购买ES实例后，发现所选的配置不符合预期时，可以参考下表，匹配您的配置信息，选择合适的解决方案。

**警告：** 在下表解决方案中，如果涉及到退订，请在退订前先备份数据（[手动备份与恢复](/cn.zh-CN/Elasticsearch/数据备份/手动备份与恢复.md)）。退订后数据会被清除，无法恢复。

|配置|解决方案|
|--|----|
|付费模式|如果您购买的是按量付费的实例，可转换为包年包月，详情请参见[按量付费转包年包月](/cn.zh-CN/产品计费/转换计费方式/按量付费转包年包月.md)。

 如果您购买的是包年包月的实例，可转换为按量付费，详情请参见[包年包月转按量付费](/cn.zh-CN/产品计费/转换计费方式/包年包月转按量付费.md)。 |
|版本|实例需要满足以下任意一种情况，才支持进行版本变更： -   购买的实例版本为5.5.3，需要变更版本为5.6.16。
-   购买的实例版本为5.6.16，需要变更版本为6.3.2。
-   购买的实例版本为6.3.2，需要变更版本为6.7.0。

 实例版本升级，详情请参见[升级版本](/cn.zh-CN/Elasticsearch/版本升级/升级版本.md)。不满足上述情况的版本变更，建议退订后重新购买。 |
|地域|不支持变更，建议退订后重新购买。|
|可用区|可迁移可用区，详情请参见[迁移可用区节点](/cn.zh-CN/Elasticsearch/数据迁移/迁移可用区节点.md)。 **说明：** 在迁移可用区时，请确保ES实例已创建成功，即实例状态为正常。 |
|可用区数量|不支持变更，建议退订后重新购买。|
|实例规格|支持变更，详情请参见[升配集群](/cn.zh-CN/Elasticsearch/升降配实例/升配集群.md)和[降配集群](/cn.zh-CN/Elasticsearch/升降配实例/降配集群.md)。|
|存储类型|支持变更，详情请参见[升配集群](/cn.zh-CN/Elasticsearch/升降配实例/升配集群.md)和[降配集群](/cn.zh-CN/Elasticsearch/升降配实例/降配集群.md)。|
|云盘加密|不支持变更，建议退订后重新购买。|
|单节点存储空间|支持变更，详情请参见[升配集群](/cn.zh-CN/Elasticsearch/升降配实例/升配集群.md)。|
|数据节点数量|支持变更，详情请参见[升配集群](/cn.zh-CN/Elasticsearch/升降配实例/升配集群.md)和[降配集群](/cn.zh-CN/Elasticsearch/升降配实例/降配集群.md)。|
|网络类型、专有网络、虚拟交换机|不支持变更，建议退订后重新购买。 **说明：** 目前只支持专有网络。 |
|登录名|默认的管理员账号为elastic，不支持更改。您也可以在Kibana中创建用户，并为该用户授予对应的权限，详情请参见[创建角色](/cn.zh-CN/访问控制/Kibana角色管理/创建角色.md)和[创建用户](/cn.zh-CN/访问控制/Kibana角色管理/创建用户.md)。|
|登录密码|支持变更，详情请参见[重置实例访问密码](/cn.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|

以上未提到的配置，请在集群升配页面或者降配页面进行查验，详情请参见[升配集群](/cn.zh-CN/Elasticsearch/升降配实例/升配集群.md)和[降配集群](/cn.zh-CN/Elasticsearch/升降配实例/降配集群.md)。

## ES购买页的版本具体对应的是哪个版本？

|购买页版本|具体版本|
|-----|----|
|7.10|7.10.0|
|7.7|7.7.1|
|7.4|7.4.0|
|6.8|6.8.6|
|6.7|6.7.0|
|6.3|6.3.2|
|5.6|5.6.16|
|5.5|5.5.3|

购买时，如果您已拥有自建集群，建议您选择相近的版本，比如小版本相近；如果没有，建议您选择最新版本。

## 购买ES实例时，专有网络为空，如何处理？

您需要确认创建的RAM账号是否拥有对应的权限，建议对RAM账号授予**AliyunElasticsearchFullAccess**和**AliyunVPCFullAccess**权限。详情请参见[为RAM用户授权](/cn.zh-CN/访问控制/为RAM用户授权.md)。

## 购买ES实例时，已拥有对应的专有网络，但无法选择虚拟交换机或虚拟交换机为空，如何解决？

您需要确认对应区域下是否创建了虚拟交换机，如果没有创建，详情请参见[搭建IPv4专有网络](/cn.zh-CN/快速入门/搭建IPv4专有网络.md)。

## 已购买的实例退订后重新购买，实例的访问地址会变吗？

会变化。建议您购买新实例后，先修改对应的客户端代码，再退订旧实例，保证业务不间断。

## 如何释放ES实例？

可在实例列表页面单击**更多** \> **释放实例**，详情请参见[释放实例](/cn.zh-CN/Elasticsearch/实例管理/释放实例.md)。

## ES实例停止服务后多久被释放？

停止服务1天后释放实例，释放后数据将被永久删除，无法恢复。更多注意事项请参见[欠费与停机](/cn.zh-CN/产品计费/欠费与停机.md)。

## 我可以购买单机版的ES实例吗？

不支持，购买时至少需要选择两个数据节点，详情请参见[购买页面参数（商业版）](/cn.zh-CN/Elasticsearch/快速购买/购买页面参数（商业版）.md)。

## 购买实例时，资源已经售罄怎么办？

如果在创建实例时遇到资源售罄的情况，建议您采取以下措施：

-   更换地域
-   更换可用区
-   更换资源配置

如果调整需求后仍然没有资源，建议您等待一段时间再购买。实例资源是动态的，如果资源不足，阿里云会尽快补充资源，但是需要一定时间。

## ES支持版本升级或降级吗？

支持升级，不支持降级。升级功能目前只支持5.5.3版本升级到5.6.16版本、5.6.16版本升级到6.3.2版本、6.3.2版本升级到6.7.0版本，暂不支持其他版本间的升级，详情请参见[升级版本](/cn.zh-CN/Elasticsearch/版本升级/升级版本.md)。

如果需要其他版本间的升级或者降版本，请先购买符合需求的ES实例，同步数据到新实例中，再申请退订旧实例。

## ES支持通过SSH登录集群修改配置吗？

不支持。为确保安全性，ES不支持通过SSH登录集群。如果您需要修改集群配置，可通过ES的集群配置功能实现，详情请参见[集群配置概述](/cn.zh-CN/Elasticsearch/集群配置/集群配置概述.md)。

## 6.7版本的Logstash和6.3版本的ES能够兼容吗？

可以兼容。更多兼容性说明请参见[产品兼容性](/cn.zh-CN/产品简介/产品兼容性.md)。

## Quick BI支持ES数据源吗？

不支持。您可以通过Kibana进行数据分析与展示，详情请参见[步骤五：创建Kibana流量监控大图](/cn.zh-CN/最佳实践/Elasticsearch应用/集群管理/通过RollUp实现流量汇总最佳实践.md)。您也可以使用DataV进行可视化展示，详情请参见[使用DataV大屏展示阿里云Elasticsearch数据](/cn.zh-CN/Elasticsearch/可视化控制/使用DataV大屏展示阿里云Elasticsearch数据.md)。

## ES支持评分插件吗？

ES支持通过索引创建分词器进行数据搜索，同时也支持评分排序，详情请参见[步骤五：搜索数据](/cn.zh-CN/Elasticsearch/快速入门.md)。

## ES支持LDAP功能吗？

目前ES产品侧暂不支持LDAP功能。如果您需要使用LDAP协议对接ES进行认证，需要先在本地搭建对应ES版本集群环境进行测试，再将正确的配置提供给阿里云ES技术工程师进行配置，详情请参见[X-Pack集成LDAP认证最佳实践](/cn.zh-CN/最佳实践/Elasticsearch应用/集群管理/X-Pack集成LDAP认证最佳实践.md)。

## ES有Java SDK吗？

有的，不同的ES版本对应不同的SDK，具体使用方式请参见[Java API](/cn.zh-CN/开发指南/Java API/概述.md)。

## ES实例的内核版本在哪里查看？

ES实例的内核默认为最新版本，版本说明请参见[内核版本发布记录](/cn.zh-CN/AliES内核/内核版本发布记录.md)。如果不是最新版本，在实例的[基本信息](/cn.zh-CN/Elasticsearch/实例管理/查看实例的基本信息.md)页面会提示**有可更新的内核补丁**，单击此提示，可查看实例当前的内核版本。

![查看内核版本](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0259775061/p183995.png)

## 阿里云ES支持本地部署吗？

支持。请前往[本地部署版产品首页](https://www.aliyun.com/activity/bigdata/markets/aliyun/act/espre)，查看具体信息。单击**立即咨询**，填写相关信息，我们会在2个工作日内与您联系。

## 重启ES实例或节点需要多久？

在重启ES实例或节点时，页面上会显示预估时间，这个时间是根据您的集群规格、数据结构和大小等进行评估的。重启实例耗时较长，一般在小时级别，详情请参见[重启实例或节点](/cn.zh-CN/Elasticsearch/实例管理/重启实例或节点.md)。

## 打开或关闭ES实例的公网访问时，会触发实例重启吗？

不会。但是会有短暂的生效状态变更，不影响正常使用。

## 使用ES实例时，一部分节点的CPU和负载正常，另一部分处于空闲状态，如何处理？

此现象是集群负载不均问题引起的。导致阿里云ES集群负载不均问题的原因很多，目前主要包括shard设置不合理、segment大小不均、冷热数据需求、负载均衡及多可用区架构部署的长连接不释放等。请根据具体现象进行排查，详情请参见[集群负载不均问题的分析方法及解决方案]()。

**说明：** 在排查问题前，请先查看您的集群规格，如果为1核2 GB（测试规格），请先将规格升配只2核4 GB或以上，详情请参见[升配集群](/cn.zh-CN/Elasticsearch/升降配实例/升配集群.md)。

## 使用ES前，如何合理规划集群的资源和规格以及shard的大小和数量？

参见[规格容量评估](/cn.zh-CN/Elasticsearch/快速购买/规格容量评估.md)，对集群的规格容量进行初始规划，以此为依据来购买或者升配集群。

## 如何查看ES实例的配置参数？

可在实例的基本信息页面查看，详情请参见[查看实例的基本信息](/cn.zh-CN/Elasticsearch/实例管理/查看实例的基本信息.md)。

当您使用Transport Client访问ES实例时，`cluster.name`为实例ID，详情请参见[Transport Client（5.x）](/cn.zh-CN/开发指南/Java API/Transport Client（5.x）.md)。

## 变更集群配置会影响ES服务吗？

变更集群配置会导致集群重启。目前阿里云ES集群重启是采用滚动重启的方式，在集群状态正常（绿色）、索引至少包含1个副本的情况下，如果资源使用率也不是特别高（可在[集群监控](/cn.zh-CN/Elasticsearch/集群监控报警/查看集群监控.md)页面查看，例如节点CPU使用率为80%左右，节点HeapMemory使用率为50%左右，节点load\_1m低于当前数据节点的CPU核数），那么集群在重启期间能够持续提供服务。但建议在业务低峰期进行操作。

## ES实例支持变更云盘类型吗？

不支持。如果需要变更，可重新购买一个实例 ，进行数据迁移后，再将原实例释放。数据迁移的具体步骤请参见[设置跨集群OSS仓库](/cn.zh-CN/Elasticsearch/数据备份/设置跨集群OSS仓库.md)、[使用阿里云Logstash迁移数据到阿里云ES](/cn.zh-CN/Logstash/快速入门.md)。

## ES支持将其他类型的节点变为冷数据节点吗？

不支持，会导致实例不稳定，详情请参见[“Hot-Warm” Architecture in Elasticsearch 5.x](https://www.elastic.co/cn/blog/hot-warm-architecture-in-elasticsearch-5-x?spm=a2c4g.11186623.2.14.22557ad9E8iQ2N)。

## 升级了实例规格后，可以降低配置吗，如何操作？

支持节点缩容，暂时不支持实例规格降配，详情请参见[缩容集群数据节点](/cn.zh-CN/Elasticsearch/升降配实例/缩容集群数据节点.md)。

## 业务量临时突增，如何变更集群配置，来保证业务正常进行？

在业务量临时突增的情况下，建议您先进行节点扩容（[升配集群](/cn.zh-CN/Elasticsearch/升降配实例/升配集群.md)），之后再进行节点缩容（[缩容集群数据节点](/cn.zh-CN/Elasticsearch/升降配实例/缩容集群数据节点.md)）。集群数据节点扩缩容都需要重启集群才能生效，在重启前，请注意：

-   确保实例的状态为正常（绿色）。
-   索引至少包含1个副本、 资源使用率不是很高（可在集群监控页面查看，例如节点CPU使用率为80%左右，节点HeapMemory使用率为50%左右，节点load\_1m低于当前数据节点的CPU核数）。

## 升配集群时，提示UpgradeVersionMustFromConsole如何处理？

出现此问题的原因是版本变更不符合要求。目前阿里云ES只支持5.5.3版本升级到5.6.16版本、5.6.16版本升级到6.3.2版本、6.3.2版本升级到6.7.0版本，暂不支持其他版本间的升级。

## 升级ES版本需要多长时间？

具体时长与您集群中的数据大小、数据结构、集群规格等都有关系。一般为1个小时左右。

## 升级ES版本会影响集群服务吗？

升级期间可以继续向集群写入数据或从集群读取数据，但不能进行其他变更操作，建议在流量低峰期进行。升级的注意事项和操作步骤请参见[升级版本](/cn.zh-CN/Elasticsearch/版本升级/升级版本.md)。

## 是否可以在集群的YML文件配置中，调整http.max\_content\_length和discovery.zen.ping\_timeout值？

阿里云ES暂不支持用户自行配置这两个参数，如需添加请联系阿里云ES技术工程师。添加前，请确认参数的正确性，以及参数修改带来的影响。如果参数不正确，集群将无法正常滚动重启。

**说明：** discovery.zen.ping\_timeout、discovery.zen.fd.ping\_timeout、discovery.zen.fd.ping\_interval、discovery.zen.fd.ping\_retries参数通常是不需要调整的。

## 我可以切换ES实例的VPC吗？

不支持切换。您可以重新购买一个对应VPC下的ES实例，然后迁移数据，再释放原实例。

## ES支持修改JVM参数吗？

阿里云ES的JVM参数使用的是官方ES推荐的参数配置，默认为集群内存的一半，不建议更改。

## 使用IK分词器时，如何自定义扩展分词词典内容？

您可以通过阿里云ES的IK分词插件的冷更新或热更新功能，添加或删除词典中的内容，详情请参见[使用IK分词插件（analysis-ik）](/cn.zh-CN/Elasticsearch/插件配置/系统默认插件/使用IK分词插件（analysis-ik）.md)。

## 使用IK分词插件时，提示ik startOffset报错，如何处理？

以上报错触发了ES 6.7版本的bug，需要您重启集群，详情请参见[重启实例或节点](/cn.zh-CN/Elasticsearch/实例管理/重启实例或节点.md)。

## 本地IK词库文件丢失，可以在集群管理页面找回吗？

无法找回，只能在集群管理页面上进行删除和更新操作。建议您下载官方[主分词和停用分词](https://github.com/medcl/elasticsearch-analysis-ik/releases?spm=a2c4g.11186623.2.14.29f41736MORIEU)文件，把对应的主分词和停用词改成您系统词库中的名词，然后重新上传。

## 更新IK分词词库后，如何使新的词库对之前的数据生效？

需要重建索引。已经配置了IK分词的索引，在IK词典冷更新或热更新操作完成后将只对新数据生效。如果您希望对全部数据生效，需要重建索引，详情请参见[配置reindex白名单](/cn.zh-CN/Elasticsearch/集群配置/配置YML参数.md)。

## FullGC有标准值吗？

FullGC（清理整个堆空间）是否存在问题，需要通过业务延时，以及对比历史和现在的状况来分析。CMS回收器在内存为75%就会开始回收，需要留一点空间以应付突增流量。

## 不使用的插件可以卸载吗？

部分可卸载。可在系统默认插件列表中查看，如果插件对应的操作列下出现卸载，则表示该插件可卸载，具体操作步骤请参见[安装或卸载系统默认插件](/cn.zh-CN/Elasticsearch/插件配置/安装或卸载系统默认插件.md)。

## 阿里云ES的IK分词插件和开源版本的IK插件的词库一致吗？

一致。阿里云ES IK插件内置的词库就是对应版本的开源IK的词库，详情请参见[IK Analysis for Elasticsearch](https://github.com/medcl/elasticsearch-analysis-ik)。

## 自定义插件可以访问外部网络吗，例如读取Github上的词库文件？

自定义插件不支持访问外网。如果要访问外部文件，可将文件上传至阿里云OSS上，通过连接OSS读取文件。

## 自定义插件支持热更新功能吗？

不支持。如果需要热更新，可参考IK词典热更新的方式，自行配置，详情请参见[IK Analysis for Elasticsearch](https://github.com/medcl/elasticsearch-analysis-ik)。

## analysis-aliws分词是如何配置的，文件的格式是什么样的？

具体配置方式请参见[使用AliNLP分词插件（analysis-aliws）](/cn.zh-CN/Elasticsearch/插件配置/系统默认插件/使用AliNLP分词插件（analysis-aliws）.md)。

词典文件要求如下：

-   文件名：必须是aliws\_ext\_dict.txt。
-   文件格式：必须是UTF-8格式。
-   内容：每行一个词，前后不能有空白字符；需要使用UNIX或Linux的换行符，即每行结尾是\\n。如果是在Windows系统中生成的文件，需要在Linux机器上使用dos2unix工具将词典文件处理后再上传。

## ES同义词、IK分词、AliNLP分词有哪些区别？

|分词类型|使用方式|功能描述|支持上传的文件类型|分词器或分析器|
|----|----|----|---------|-------|
|同义词|在集群配置模块，上传同义词文件后使用。|在文件中写入几个同义词，查询其中一个，其他的也会显示。|UTF-8编码的TXT文件|自定义|
|IK分词|analysis-ik插件方式。|根据main.dic文件，对一段话进行拆分。查询时，只要查询的内容中包含了拆分后的词，查询结果中就会显示该段话。同时还包含了停用词stop.dic，拆分后，stop.dic文件中包含的词会被过滤掉。对应的词库可以在[官方文档](https://github.com/medcl/elasticsearch-analysis-ik)中查看。|UTF-8编码的DIC文件|分词器： -   ik\_smart
-   ik\_max\_word |
|AliNLP分词|analysis-aliws插件方式。|与IK分词大致相同，但不包含单独的停用词文件。停用词集成在主分词词库：aliws\_ext\_dict.txt文件中，且词库不对外开放。目前不支持自定义停用词。|文件名必须为：aliws\_ext\_dict.txt，UTF-8编码|-   分析器：aliws（不会截取虚词、虚词短语、符号）
-   分词器：aliws\_tokenizer |

## ES支持设置.security日志的保存时间吗？

不支持。阿里云ES不支持自动过期清除策略，因此请手动清除过期的.security索引，详情请参见[（可选）步骤六：删除索引](/cn.zh-CN/Elasticsearch/快速入门.md)。

## ES只能看到7天内的日志，如何查看更多的历史日志？

可调用ListsearchLog API获取更多历史日志，详情请参见[ListSearchLog](/cn.zh-CN/API参考/Elasticsearch/日志查询/ListSearchLog.md)。

## 查看不到ES的查询和更新日志，如何处理？

可通过设置慢日志，降低日志记录的时间戳查看，详情请参见[配置慢日志](/cn.zh-CN/Elasticsearch/查询日志.md)。

## 如何配置和查看ES实例的慢日志？

默认情况下，ES实例的慢日志会记录5～10秒的读写操作，登录该实例的[Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)，执行相关命令，降低日志记录的时间戳，以抓取更多的日志，详情请参见[配置慢日志](/cn.zh-CN/Elasticsearch/查询日志.md)。

**说明：** 不支持修改慢日志格式。

## 如何通过程序定期获取ES实例的慢日志？

可调用阿里云ES的ListSearchLog API实现，详情请参见[ListSearchLog](/cn.zh-CN/API参考/Elasticsearch/日志查询/ListSearchLog.md)。

## ES实例的快照能恢复到其他版本的实例中吗？

如果是自动备份，则只能将快照恢复到原实例中，详情请参见[自动备份与恢复](/cn.zh-CN/Elasticsearch/数据备份/自动备份与恢复.md)。

如果是手动备份，可以将快照恢复到其他实例中。一般是建议在相同版本之间恢复，不同版本之间可能存在兼容性问题，详情请参见[手动备份与恢复](/cn.zh-CN/Elasticsearch/数据备份/手动备份与恢复.md)。

## 备份ES数据时，提示集群状态不健康，如何处理？

ES集群状态不健康时，不能使用阿里云ES提供的自动备份和跨集群OSS仓库设置功能。此时您可以购买一个与ES实例相同区域的OSS Bucket，手动创建仓库进行快照备份，详情请参见[手动备份与恢复](/cn.zh-CN/Elasticsearch/数据备份/手动备份与恢复.md)。

## 开启了自动备份，但是没有设置过OSS，是不是没有备份成功？

阿里云ES默认为您提供了一个OSS Bucket，您可以参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)，在Kibana中使用`GET _snapshot/aliyun_auto_snapshot/_all`命令，获取自动备份的数据。

## 通过快照迁移（恢复）数据时，目标端显示分片异常。通过分片恢复指令`POST /_cluster/reroute?retry_failed=true`依旧无法成功，且对应索引显示异常，如何处理？

快照迁移（恢复）数据时，出现如下异常情况：

![快照恢复问题](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5750644061/p179003.png)

尝试删除问题索引后，再通过\_restore API进行恢复。恢复命令中需要添加max\_restore\_bytes\_per\_sec参数，限制节点的恢复速度，默认为每秒40 MB。

```
POST /_snapshot/aliyun_snapshot_from_instanceId/es-cn-instanceId_datetime/_restore
{
    "indices": "myIndex",
    "settings": {
    "max_restore_bytes_per_sec" : "150mb" 
    }
}
```

**说明：** 您还可以添加其他参数说明，例如：

-   compress：是否压缩，默认为true。
-   max\_snapshot\_bytes\_per\_sec：每个节点的快照速率，默认为每秒40 MB。

## 阿里云ES中的数据是否可以导出到本地？

阿里云ES提供了数据备份功能，详情请参见[数据备份概述](/cn.zh-CN/Elasticsearch/数据备份/数据备份概述.md)。您可以先将数据备份到OSS中，再通过OSS的[下载文件](/cn.zh-CN/快速入门/控制台快速入门/下载文件.md)功能将数据存储到本地。

## 如何使用X-Pack Watcher的邮件提醒功能？

可通过配置Watcher的Actions来实现，详情请参见[Watcher settings in Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.x/actions-email.html)。

**说明：** 阿里云ES的Watcher功能不支持直接与公网进行通讯，需要基于实例的内网地址来进行通讯。如果您需要使用XPack Watcher，还需要购买一台能同时访问公网和ES实例的ECS实例，作为代理去执行Actions，详情请参见[配置X-Pack Watcher](/cn.zh-CN/Elasticsearch/集群监控报警/配置X-Pack Watcher.md)。

## 出现GC内存无法分配的报警，如何处理？

可能原因包括：集群负载过高、查询QPS太高或者写入数据太大等，请按照以下说明排查处理：

-   集群负载过高：参见[集群磁盘使用率过高和read\_only问题的排查与处理方法]()处理。
-   查询QPS太高或者写入数据太大：建议安装集群限流插件（aliyun-qos），使用aliyun-qos插件进行读写限流，详情请参见[使用集群限流插件（aliyun-qos）](/cn.zh-CN/Elasticsearch/插件配置/系统默认插件/使用集群限流插件（aliyun-qos）.md)。

    **说明：** 对于图片检索，建议安装向量检索插件（aliyun-knn），并参见[使用向量检索插件（aliyun-knn）]()进行集群和索引规划。


## ES是否支持Grafana监控？

目前，仅支持6.7.0版本的实例，且内核版本为1.2.0及以上。详细信息，请参见[查看监控指标](/cn.zh-CN/高级监控报警/可视化监控/指标监控/查看监控指标.md)。

## 如何使用客户端连接阿里云ES集群，与开源ES有什么区别？

使用阿里云ES的内网或外网地址连接，对应开源ES的集群地址，详情请参见[通过客户端访问阿里云Elasticsearch](/cn.zh-CN/开发指南/通过客户端访问阿里云Elasticsearch.md)。

## 通过客户端访问ES实例时，可以关闭Basic Auth（安全认证）吗？

不可以。Basic Auth是X-pack自带的Kibana认证机制，ES实例包含X-Pack功能，因此不支持关闭Basic Auth。

## 阿里云ECS和ES的VPC相同，但可用区不同，那ECS可以通过内网访问ES吗？

可以。只要ECS和ES在同一VPC下，就可以通过内网访问。

## 如何通过外网连接ES实例？

可通过实例的公网地址来连接，并且需要配置公网地址访问白名单，详情请参见[配置Elasticsearch公网或私网访问白名单](/cn.zh-CN/Elasticsearch/安全配置/配置Elasticsearch公网或私网访问白名单.md)。连接时需要配置访问域名、用户名和密码等参数，详情请参见[通过客户端访问阿里云Elasticsearch](/cn.zh-CN/开发指南/通过客户端访问阿里云Elasticsearch.md)。

