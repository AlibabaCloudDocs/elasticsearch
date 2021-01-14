---
keyword: [analysis-aliws插件, AliNLP分词插件, es分词]
---

# 使用AliNLP分词插件（analysis-aliws）

AliNLP分词插件是阿里云Elasticsearch自带的一个系统默认插件。通过该插件，您可以在阿里云Elasticsearch中集成对应的分析器和分词器，完成文档分析和检索。您也可以使用AliNLP分词插件的词库配置功能，通过词典文件的热更新自定义词库配置（替换默认词典）。本文介绍如何使用AliNLP分词插件。

已安装AliNLP分词插件（英文名为analysis-aliws，默认未安装）。

如果还未安装，请先安装AliNLP分词插件。安装前需要确保实例的内存大小为4GB及以上（生产环境中要求最低为8GB）。具体安装步骤，请参见[安装或卸载系统默认插件](/cn.zh-CN/Elasticsearch/插件配置/安装或卸载系统默认插件.md)。

**说明：**

-   5.x版本的实例不支持AliNLP分词插件。
-   如果实例的内存大小不满足要求，需要先升级。具体操作，请参见[升配集群](/cn.zh-CN/Elasticsearch/升降配实例/升配集群.md)。

AliNLP分词插件安装成功后，阿里云Elasticsearch默认会集成以下分析器和分词器：

-   分析器：aliws（不会截取虚词、虚词短语、符号）
-   分词器：aliws\_tokenizer

您可以使用上述分析器和分词器查询文档，也可以通过词库配置功能，自定义更新分词词库。详细信息，请参见下文的[查询文档](#section_t5f_ueu_6rx)和[配置词库](#section_v52_vxj_2ph)。

## 查询文档

1.  登录对应阿里云Elasticsearch实例的Kibana控制台。

    具体操作，请参见[登录Kibana控制台](/cn.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**（开发工具）。

3.  在**Console**中，执行如下命令创建索引。

    -   7.0以下版本

        ```
        PUT /index
        {
            "mappings": {
                "fulltext": {
                    "properties": {
                        "content": {
                            "type": "text",
                            "analyzer": "aliws"
                        }
                    }
                }
            }
        }
        ```

    -   7.0及以上版本

        ```
        PUT /index
        {
          "mappings": {
            "properties": {
                "content": {
                    "type": "text",
                    "analyzer": "aliws"
                  }
              }
          }
        }
        ```

    以上示例创建了一个名称为`index`的索引，类型为`fulltext`（7.x版本为`_doc`）。包含了一个`content`属性，类型为`text`，并添加了`aliws`分析器。

    执行成功后，返回如下结果。

    ```
    {
      "acknowledged": true,
      "shards_acknowledged": true,
      "index": "index"
    }
    ```

4.  执行如下命令，添加文档。

    **说明：** 如下命令仅适用于Elasticsearch 7.0以下版本，7.0及以上版本需要将`fulltext`修改为`_doc`。

    ```
    POST /index/fulltext/1
    {
      "content": "I like go to school."
    }
    ```

    以上示例创建了名称为`1`的文档，并设置了文档中的`content`字段的内容为`I like go to school.`。

    执行成功后，返回如下结果。

    ```
    {
      "_index": "index",
      "_type": "fulltext",
      "_id": "1",
      "_version": 1,
      "result": "created",
      "_shards": {
        "total": 2,
        "successful": 2,
        "failed": 0
      },
      "_seq_no": 0,
      "_primary_term": 1
    }
    ```

5.  执行如下命令，查询文档。

    **说明：** 如下命令仅适用于Elasticsearch 7.0以下版本，7.0及以上版本需要将`fulltext`修改为`_doc`。

    ```
    GET /index/fulltext/_search
    {
      "query": {
        "match": {
          "content": "school"
        }
      }
    }
    ```

    以上示例在所有`fulltext`类型的文档中，使用`aliws`分析器，搜索`content`字段中包含`school`的文档。

    执行成功后，返回如下结果。

    ```
    {
      "took": 5,
      "timed_out": false,
      "_shards": {
        "total": 5,
        "successful": 5,
        "skipped": 0,
        "failed": 0
      },
      "hits": {
        "total": 1,
        "max_score": 0.2876821,
        "hits": [
          {
            "_index": "index",
            "_type": "fulltext",
            "_id": "2",
            "_score": 0.2876821,
            "_source": {
              "content": "I like go to school."
            }
          }
        ]
      }
    }
    ```


**说明：** 如果您在使用AliNLP分词插件时，得到的结果不符合预期，可通过下文的[测试分析器](#section_nrc_ex9_wo2)和[测试分词器](#section_kwf_gcz_2la)排查调试。

## 配置词库

AliNLP分词插件支持词库配置，即上传自定义词典文件，替换默认词典。上传后节点能自动加载词典文件，实现词典的热更新操作（不会触发集群重启）。

1.  登录[阿里云Elasticsearch控制台](https://elasticsearch.console.aliyun.com/#/home)。

2.  在左侧导航栏，单击**Elasticsearch实例**。

3.  在顶部菜单栏处，选择资源组和地域，然后在**实例列表**中单击目标实例ID。

4.  在左侧导航栏，单击**插件配置**。

5.  在**系统默认插件列表**中，单击**analysis-aliws**插件右侧**操作**列下的**词库配置**。

6.  在**词库配置**页面下方，单击**配置**。

7.  选择词典文件的上传方式，并按照以下说明上传词典文件。

    ![更新ALIWS分词词库](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5846359951/p103059.png)

    词典文件要求如下：

    -   文件名：必须是aliws\_ext\_dict.txt。
    -   文件格式：必须是UTF-8格式。
    -   内容：每行一个词，前后不能有空白字符；需要使用UNIX或Linux的换行符，即每行结尾是`\n`。如果是在Windows系统中生成的文件，需要在Linux机器上使用dos2unix工具将词典文件处理后再上传。
    您可以通过**Text文件**和**添加OSS文件**两种方式上传词典文件：

    -   **Text文件**：单击**上传txt文件**，选择一个本地文件进行上传。
    -   **添加OSS文件**：输入Bucket名称和文件名称，单击**添加**。

        请确保Bucket与阿里云Elasticsearch实例在同一区域下。且源端（OSS）的文件内容发生变化后，需要重新上传词典文件才能生效，不支持自动同步更新。

8.  单击**保存**。

    保存后，不会触发集群重启，但会触发集群变更使词典文件生效，此过程需要10分钟左右。


## 测试分析器

执行如下命令，测试aliws分析器。

```
GET _analyze
{
  "text": "I like go to school.",
  "analyzer": "aliws"
}
```

执行成功后，返回如下结果。

```
{
  "tokens" : [
    {
      "token" : "i",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "like",
      "start_offset" : 2,
      "end_offset" : 6,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "go",
      "start_offset" : 7,
      "end_offset" : 9,
      "type" : "word",
      "position" : 4
    },
    {
      "token" : "school",
      "start_offset" : 13,
      "end_offset" : 19,
      "type" : "word",
      "position" : 8
    }
  ]
}
```

## 测试分词器

执行如下命令，测试aliws\_tokenizer分词器。

```
GET _analyze
{
  "text": "I like go to school.",
  "tokenizer": "aliws_tokenizer"
}
```

执行成功后，返回如下结果。

```
{
  "tokens" : [
    {
      "token" : "I",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : " ",
      "start_offset" : 1,
      "end_offset" : 2,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "like",
      "start_offset" : 2,
      "end_offset" : 6,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : " ",
      "start_offset" : 6,
      "end_offset" : 7,
      "type" : "word",
      "position" : 3
    },
    {
      "token" : "go",
      "start_offset" : 7,
      "end_offset" : 9,
      "type" : "word",
      "position" : 4
    },
    {
      "token" : " ",
      "start_offset" : 9,
      "end_offset" : 10,
      "type" : "word",
      "position" : 5
    },
    {
      "token" : "to",
      "start_offset" : 10,
      "end_offset" : 12,
      "type" : "word",
      "position" : 6
    },
    {
      "token" : " ",
      "start_offset" : 12,
      "end_offset" : 13,
      "type" : "word",
      "position" : 7
    },
    {
      "token" : "school",
      "start_offset" : 13,
      "end_offset" : 19,
      "type" : "word",
      "position" : 8
    },
    {
      "token" : ".",
      "start_offset" : 19,
      "end_offset" : 20,
      "type" : "word",
      "position" : 9
    }
  ]
}
```

