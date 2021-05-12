# Search for data

This topic describes how to use Alibaba Cloud Elasticsearch to search for data. Elasticsearch supports two search methods: full-text search and search by condition.

## Full-text search

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Access and configure an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Quick Start/Access and configure an Elasticsearch cluster.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab, run the following command to search for products with descriptions that contain `Daily push messages when returns are credited to your account`.

    ```
    GET /product_info/products/_search
    {
      "query": {
        "match": {
          "describe": "Daily push messages when returns are credited to your account"
        }
      }
    }
    ```

    If the command is executed successfully, the following result is returned:

    ```
    {
      "took" : 21,
      "timed_out" : false,
      "_shards" : {
        "total" : 5,
        "successful" : 5,
        "skipped" : 0,
        "failed" : 0
      },
      "hits" : {
        "total" : 6,
        "max_score" : 1.3968885,
        "hits" : [
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "WLvWYXAB8Rql5AUxLqUU",
            "_score" : 1.3968885,
            "_source" : {
              "productName" : "Western Tongbao",
              "annual_rate" : "3.1100%",
              "describe" : "90-day wealth management product. Minimum investment of CNY 10,000. Daily push messages when returns are credited to your account."
            }
          },
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "WrvWYXAB8Rql5AUxLqUU",
            "_score" : 1.3968885,
            "_source" : {
              "productName" : "Monthly 5G Device Purchase Profit",
              "annual_rate" : "3.1200%",
              "describe" : "90-day wealth management product. Minimum investment of CNY 12,000. Daily push messages when returns are credited to your account."
            }
          },
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "WbvWYXAB8Rql5AUxLqUU",
            "_score" : 1.3547361,
            "_source" : {
              "productName" : "Anxiang Livestock Industry",
              "annual_rate" : "3.3500%",
              "describe" : "270-day wealth management product. Minimum investment of CNY 40,000. Daily push messages when returns are immediately credited to your account."
            }
          },
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "W7vWYXAB8Rql5AUxLqUU",
            "_score" : 1.1507283,
            "_source" : {
              "productName" : "New Energy Power Wealth Management",
              "annual rate" : "3.0100%",
              "describe" : "30-day wealth management product. Minimum investment of CNY 8,000. Daily push messages for returns."
            }
          },
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "XLvWYXAB8Rql5AUxLqUU",
            "_score" : 0.5753642,
            "_source" : {
              "productName" : "Microcredit Profit",
              "annual_rate" : "2.7500%",
              "describe" : "3-day popular wealth management product. No service fees. Minimum investment of CNY 500. Push messages for returns."
            }
          },
          {
            "_index" : "product_info",
            "_type" : "products",
            "_id" : "V7vWYXAB8Rql5AUxLqUU",
            "_score" : 0.31854028,
            "_source" : {
              "productName" : "Daily Wealth Management for Comprehensive Health",
              "annual_rate" : "3.2200%",
              "describe" : "180-day wealth management product. Minimum investment of CNY 20,000. Low-risk investment. Select whether to receive push messages for returns."
            }
          }
        ]
      }
    }
    ```

    Alibaba Cloud Elasticsearch allows you to create a tokenizer based on indexes to search for data. You can also sort search results by score. In the preceding result, the descriptions of the first two products contain `Daily push messages when returns are credited to your account`, while the descriptions of the last two products contain only `push messages`. The higher the ranking of a product in the search result, the higher the matching degree and score of the product.


## Search by condition

Run the following command to search for products with an annualized rate of 3.0000% to 3.1300%.

```
GET /product_info/products/_search
{
  "query": {
    "range": {
      "annual_rate": {
        "gte": "3.0000%",
        "lte": "3.1300%"
      }
    }
  }
}
```

If the command is executed successfully, the following result is returned:

```
{
  "took" : 10,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 2,
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "product_info",
        "_type" : "products",
        "_id" : "WLvWYXAB8Rql5AUxLqUU",
        "_score" : 1.0,
        "_source" : {
          "productName" : "Western Tongbao",
          "annual_rate" : "3.1100%",
          "describe" : "90-day wealth management product. Minimum investment of CNY 10,000. Daily push messages when returns are credited to your account."
        }
      },
      {
        "_index" : "product_info",
        "_type" : "products",
        "_id" : "WrvWYXAB8Rql5AUxLqUU",
        "_score" : 1.0,
        "_source" : {
          "productName" : "Monthly 5G Device Purchase Profit",
          "annual_rate" : "3.1200%",
          "describe" : "90-day wealth management product. Minimum investment of CNY 12,000. Daily push messages when returns are credited to your account."
        }
      }
    ]
  }
}
```

Elasticsearch finds products that meet requirements based on the search condition and displays the products in descending order.

For more information, see [Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/6.7/query-dsl.html).

