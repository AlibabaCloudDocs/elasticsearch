# Delete an index

This topic describes how to delete an index. You can delete indexes that you no longer use to avoid resource waste. Proceed with caution because you cannot recover indexes that have been deleted.

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Access and configure an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Quick Start/Access and configure an Elasticsearch cluster.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab, run the following command to delete index `product_info`.

    ```
    DELETE /product_info
    ```

    If the index is deleted, the following result is returned:

    ```
    {
      "acknowledged" : true
    }
    ```


