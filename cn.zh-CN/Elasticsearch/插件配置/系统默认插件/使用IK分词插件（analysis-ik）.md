---
keyword: [ik词典冷更新, ik词典热更新, es停用词]
---

# 使用IK分词插件（analysis-ik）

IK分词插件（英文名为analysis-ik）是阿里云Elasticsearch的扩展插件，默认不能卸载。该插件在开源插件的基础上，扩展支持了对象存储服务OSS（Object Storage Service）词典文件的动态加载，可以实现IK词典的冷更新和热更新。本文介绍如何使用IK分词插件。

阿里云Elasticsearch的IK分词插件支持[IK词典冷更新](#ik)和[IK词典热更新](#section_5qm_xjg_ouf)，两者区别如下。

|更新方式|生效方式|加载方式|说明|
|----|----|----|--|
|冷更新|对整个集群的词典进行更新操作，需要重启集群才能生效。|系统将上传的词典文件传送到Elasticsearch节点上，并修改IKAnalyzer.cfg.xml文件，然后重启Elasticsearch节点加载词典文件。|IK词典冷更新支持修改IK自带的主词库及停用词主词库。在IK词典冷更新页面可以看到系统自带的主词典为`SYSTEM_MAIN.dic`，系统自带的停用词主词典为`SYSTEM_STOPWORD.dic`。|
|热更新|第一次上传词典文件时，会对整个集群的词典进行更新，需要重启集群才能生效；二次上传同名文件不会触发集群重启，在运行过程中直接加载词库。|当词典文件内容发生变化时，上传词典文件后Elasticsearch节点能自动加载词典文件，实现词典的更新操作。 如果词典文件列表发生变化，例如上传新词典文件或删除词典文件，那么整个集群都需要重新加载词典的配置（因为会涉及到修改IKAnalyzer.cfg.xml文件）。

|第一次上传词典文件时，需要修改IKAnalyzer.cfg.xml文件，所以在更新词典后，需要重启集群才能生效。|

**说明：**

对于已经配置了IK分词的索引，在IK词典冷更新或热更新操作完成后将只对新数据生效。如果您希望对全部数据生效，需要重建索引。

进行IK词典冷更新时，系统不支持删除自带的主词典和停用词主词典，但可以修改。 修改方式如下：

-   如果需要修改系统自带的主词典文件，请上传以**SYSTEM\_MAIN.dic**命名的主词典，系统会自动覆盖原始内容。详细信息，请参见[IK Analysis for Elasticsearch](https://github.com/medcl/elasticsearch-analysis-ik)。
-   如果需要修改系统自带的停用词主词典文件，请上传以**SYSTEM\_STOPWORD.dic**命名的停用词主词典，系统会自动覆盖原始内容。详细信息，请参见[IK Analysis for Elasticsearch](https://github.com/medcl/elasticsearch-analysis-ik)和[停用词配置](#section_3gd_iqv_b8p)。

## 前提条件

确保集群状态为正常。可在基本信息页面查看。具体操作，请参见[查看实例的基本信息](/cn.zh-CN/Elasticsearch/实例管理/查看实例的基本信息.md)。

## IK词典冷更新

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  进入目标实例。

    1.  在顶部菜单栏处，选择资源组和地域。

    2.  在左侧导航栏单击**Elasticsearch实例**，然后在**Elasticsearch实例**中单击目标实例ID。

4.  在左侧导航栏，单击**插件配置**。

5.  在**系统默认插件列表**中，找到需要更新的IK插件，单击其右侧**操作**列下的**冷更新**。

    ![IK词典冷更新入口](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1846359951/p40216.png)

6.  在**冷更新**页面，单击下方的**配置**。

7.  在**IK主分词词库**下方，选择词典的更新方式，并按照以下说明上传词典文件。

    ![IK主分词词库](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1846359951/p40219.png)

    阿里云Elasticsearch支持**上传DIC文件**和**添加OSS文件**两种词典更新方式：

    -   **上传DIC文件**：单击**上传DIC文件**，选择一个本地文件进行上传。
    -   **添加OSS文件**：输入Bucket名称和文件名称，单击**添加**。

        请确保Bucket与当前阿里云Elasticsearch实例在同一区域下，且文件为DIC文件。当源端（OSS）的文件内容发生变化后，需要重新手动配置上传才能生效，不支持自动同步更新。

    **警告：** 以下操作会重启实例，为保证您的业务不受影响，请确认后操作。

8.  滑动到页面底端，勾选**该操作会重启实例，请确认后操作**，单击**保存**。

    无论是词典文件变化，还是词典内容发生变化，冷更新操作都会触发集群重启。

9.  重启成功后，登录对应Elasticsearch实例的Kibana控制台，执行以下命令测试词典是否生效。

    登录控制台的具体操作步骤，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

    ```
    GET _analyze
    {
    "analyzer": "ik_smart",
    "text": ["您词典中包含的词"]
    }
    ```

    **说明：** IK分词插件的分词器包括ik\_smart和ik\_max\_word，两者区别如下：

    -   ik\_max\_word：将文本按照最细粒度进行拆分。例如会将`中华人民共和国国歌`拆分为`中华人民共和国,中华人民,中华,华人,人民共和国,人民,共和国,共和,国,国歌`，适合术语查询。
    -   ik\_smart：将文本按照粗粒度进行拆分。例如会将`中华人民共和国国歌`拆分为`中华人民共和国,国歌`，适合短语查询。

## IK词典热更新

1.  在**系统默认插件列表**中，找到需要更新的IK插件，单击其右侧**操作**列下的**热更新**。

    ![IK词典热更新入口](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1846359951/p40222.png)

2.  在**热更新**页面，单击右下方的**配置**。

3.  在**IK主分词词库**下方，选择词典的更新方式，并按照以下说明上传词典文件。

    ![插件配置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2846359951/p40223.png)

    **说明：** IK热更新不支持修改系统自带的主词典，如果您需要修改系统主词典请使用IK冷更新的方式。

    阿里云Elasticsearch支持**上传DIC文件**和**添加OSS文件**两种词典更新方式：

    -   **上传DIC文件**：单击**上传DIC文件**，选择一个本地文件进行上传。
    -   **添加OSS文件**：输入Bucket名称和文件名称，单击**添加**。

        请确保Bucket与当前Elasticsearch实例在同一区域下，且文件为DIC文件（以下步骤以`dic_0.dic`文件进行说明）。且源端（OSS）的文件内容发生变化后，需要重新手动配置上传才能生效，不支持自动同步更新。

    **警告：** 以下操作会重启实例，为保证您的业务不受影响，请确认后操作。

4.  滑动到页面底端，勾选**该操作会重启实例，请确认后操作**（第一次上传词典文件，需要重启），单击**保存**。

    保存后，集群会进行滚动重启，等待滚动重启结束后，词典会自动生效。

    词典使用一段时间后，如果需要扩充或者减少词典中的内容，请继续执行以下步骤修改上传的`dic_0.dic`文件。

5.  进入词典热更新页面，先删除之前上传的同名词典文件，重新上传修改过的`dic_0.dic`同名词典文件。

    因为修改的是已存在的同名词典文件的内容，所以本次上传修改过的同名词典文件不需要滚动重启整个集群。

6.  单击**保存**。

    由于阿里云Elasticsearch节点上的插件具有自动加载词典文件的功能，所以每个节点获取词典文件的可能时间不同，请耐心等待词典生效。大概两分钟后再使用更新之后的词典，为了保证准确性，请登录Kibana控制台，多次执行以下命令进行验证。

    **说明：** 登录Kibana控制台的具体步骤，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

    ```
    GET _analyze
    {
    "analyzer": "ik_smart",
    "text": ["您词典中包含的词"]
    }
    ```


## 停用词配置

阿里云Elasticsearch默认的停用词词库配置文件中，包含了一些默认的停用词，例如：a、an、and、are、as、at、be、but、by、for、if、in、into、is、it、no、not、of、on、or、such、that、the、their、then、there、these、they、this、to、was、will、with

如果您需要去掉一些不需要的默认停用词，可通过以下步骤进行操作：

1.  下载官方Elasticsearch默认的[IK分词配置文件](https://github.com/medcl/elasticsearch-analysis-ik/releases)。

2.  解压配置文件，并打开config目录下的stopword.dic文件。

    **说明：** 如果您需要使用中文停用词，可在config目录下找到extra\_stopword.dic文件，并通过[IK词典冷更新](#ik)的方式进行上传。extra\_stopword.dic文件中包含的停用词包括：也、了、仍、从、以、使、则、却、又、及、对、就、并、很、或、把、是、的、着、给、而、被、让、在、还、比、等、当、与、于、但。

3.  删除不需要的停用词，并保存。

4.  修改stopword.dic文件名为SYSTEM\_STOPWORD.dic。

5.  使用IK词典冷更新功能，上传修改后的SYSTEM\_STOPWORD.dic文件，覆盖当前默认的停用词配置文件。

6.  确认后重启，即可更新停用词配置文件。


## 下载并更新词库文件

如果您需要对已经上传过的词库文件进行更新，可直接下载对应的文件，修改后重新上传即可。本文以冷更新的IK停用词库文件**SYSTEM\_STOPWORD.dic**为例为您展开介绍。

1.  在**系统默认插件列表**中，找到**analysis-ik**插件，单击其右侧**操作**列下的**冷更新**。

    ![IK词典冷更新入口](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1846359951/p40216.png)

2.  在**冷更新**面板中，单击**IK停用词库**下**SYSTEM\_STOPWORD.dic**文件对应的![下载按钮](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9434776261/p296767.png)图标。

    ![下载按钮位置](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1540876261/p296804.png)

3.  修改已下载的文件，并重新上传。上传操作请参见[IK词典冷更新](#ik)或者[IK词典热更新](#section_5qm_xjg_ouf)中对应的步骤。


