---
keyword: [analysis-aliws, AliNLP tokenization plug-in, Elasticsearch tokenization]
---

# Use the analysis-aliws plug-in

analysis-aliws is a built-in plug-in of Alibaba Cloud Elasticsearch. This plug-in integrates an analyzer and a tokenizer into Elasticsearch to implement document analysis and retrieval. The plug-in allows you to upload a tailored dictionary file to it. After the upload, the system automatically performs a rolling update for your cluster. Alibaba Cloud Elasticsearch clusters of V5.X do not support this plug-in.

The analysis-aliws plug-in is installed. It is not installed by default.

If it is not installed, install it. Make sure that your Elasticsearch cluster offers at least 4 GiB of memory. If your cluster is in a production environment, it must offer at least 8 GiB of memory. For more information about how to install the plug-in, see [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Install and remove a built-in plug-in.md).

**Note:** If the memory capacity of your cluster does not meet the preceding requirements, upgrade the configuration of your cluster. For more information, see [Upgrade the configuration of a cluster](/intl.en-US/Elasticsearch Instances Management/Upgrade or downgrade a cluster/Upgrade the configuration of a cluster.md).

After the analysis-aliws plug-in is installed, the following analyzer and tokenizer are integrated into your Elasticsearch cluster:

-   Analyzer: aliws, which does not return function words, function phrases, or symbols
-   Tokenizer: aliws\_tokenizer

You can use the analyzer and tokenizer to search for documents. You can also upload a tailored dictionary file to the plug-in. For more information, see [Search for a document](#section_t5f_ueu_6rx) and [Configure a dictionary](#section_v52_vxj_2ph).

## Search for a document

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to create an index:

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

    The preceding command creates an index named `index`. The type of the index is `fulltext`. The index contains the `content` property. The type of the property is `text`. The command also adds the `aliws` analyzer.

    If the command is successfully executed, the following result is returned:

    ```
    {
      "acknowledged": true,
      "shards_acknowledged": true,
      "index": "index"
    }
    ```

4.  Run the following command to add a document:

    ```
    POST /index/fulltext/1
    {
      "content": "I like go to school."
    }
    ```

    The preceding command adds a document named `1` and sets the value of the `content` field in the document to `I like go to school.`

    If the command is successfully executed, the following result is returned:

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

5.  Run the following command to search for the document:

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

    The preceding command uses the `aliws` analyzer to analyze all `fulltext`-type documents, and returns the document that has `school` contained in the `content` field.

    If the command is successfully executed, the following result is returned:

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


**Note:** If unexpected results are returned, find the cause by following the instructions provided in [Test the analyzer](#section_nrc_ex9_wo2) and [Test the tokenizer](#section_kwf_gcz_2la).

## Configure a dictionary

The analysis-aliws plug-in allows you to upload a tailored dictionary file to it. After the upload, all nodes in your Elasticsearch cluster automatically load the file. In this case, the system does not need to restart the cluster.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane, click **Plug-ins**.

5.  On the **Built-in Plug-ins** tab, find the **analysis-aliws** plug-in and click **Dictionary Configuration** in the **Actions** column.

6.  In the **Plug-ins** pane, click **Configure** in the lower-right corner.

7.  Select the method to upload the dictionary file. Then, upload the dictionary file based on the following instructions.

    ![Update the dictionary file of the analysis-aliws plug-in](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3681523061/p103059.png)

    The dictionary file must meet the following requirements:

    -   Name: aliws\_ext\_dict.txt.
    -   Encoding format: UTF-8.
    -   Content: Each row contains one word and ends with `\n` \(line break in UNIX or Linux\). No whitespace characters are used before or after this word. If the dictionary file that you want to upload is generated in Windows, you must use the dos2unix tool to convert the file before the upload.
    You can select the **TXT File** or **Add OSS File** method.

    -   **TXT File**: If you select this method, click **Upload TXT File** and select the local file that you want to upload.
    -   **Add OSS File**: If you select this method, specify Bucket Name and File Name, and click **Add**.

        Make sure that the bucket you specify resides in the same region as your Elasticsearch cluster. If the content of the dictionary that is stored in OSS changes, you must manually upload the dictionary file again.

8.  Click **Save**.

    The system does not restart your cluster but performs a rolling update to make the uploaded dictionary file take effect. The update requires about 10 minutes.


## Test the analyzer

Run the following command to test the aliws analyzer:

```
GET _analyze
{
  "text": "I like go to school.",
  "analyzer": "aliws"
}
```

If the command is successfully executed, the following result is returned:

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

## Test the tokenizer

Run the following command to test the aliws\_tokenizer tokenizer:

```
GET _analyze
{
  "text": "I like go to school.",
  "tokenizer": "aliws_tokenizer"
}
```

If the command is successfully executed, the following result is returned:

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

