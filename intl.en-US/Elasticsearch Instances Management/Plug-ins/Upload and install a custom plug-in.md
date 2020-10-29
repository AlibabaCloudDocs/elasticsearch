---
keyword: [Elasticsearch custom plug-in, upload and install a custom plug-in]
---

# Upload and install a custom plug-in

Alibaba Cloud Elasticsearch allows you to upload and install a custom or open source plug-in. This topic describes the detailed procedure.

-   The plug-in that you want to upload is prepared. Make sure that the plug-in is secure and available.

    The name of the plug-in file you upload must be 8 to 128 characters in length and can contain only uppercase letters, lowercase letters, digits, hyphens \(-\), and periods \(.\). The file name extension must be .zip.

    **Note:** Before you upload the plug-in, we recommend that you perform a test on a self-managed Elasticsearch cluster of the same version as your Alibaba Cloud Elasticsearch cluster. If the test is successful, upload the plug-in. For more information, see [Installing Plugins](https://www.elastic.co/guide/en/elasticsearch/plugins/7.9/installation.html).

-   If you want to upload a custom SQL plug-in, make sure that the `xpack.sql.enabled` parameter in the YML configuration file of your Elasticsearch cluster is set to `false`.

    For more information, see [Modify the YML file configuration](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure YML/Modify the YML file configuration.md).


1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Elasticsearch Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane of the page that appears, click **Plug-ins**.

5.  On the **Plug-ins** page, click the **Custom Plug-ins** tab. Then, click **Upload**.

    ![Upload a custom plug-in](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1667819951/p50444.png)

    **Warning:** The upload of a custom plug-in triggers a restart of the Elasticsearch cluster, and the plug-in may affect the stability of the cluster. Make sure that the custom plug-in is secure and available.

6.  In the **Upload Plug-in** dialog box, click **Select files, or drag and drop files to this area**. Then, select the custom plug-in that you want to upload and click Open.

    You can also drag and drop a custom plug-in file to this area and upload the plug-in. The following figure shows that the plug-in file elasticsearch-sql-6.7.0.0.zip is added.

    ![Upload Plug-in dialog box](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1667819951/p50445.png)

7.  Read the precautions, select the check box, and click **Upload**.

    The system then restarts the cluster. After the cluster is restarted, you can check the plug-in on the **Custom Plug-ins** tab. If the **state** of the plug-in is **Installed**, the plug-in is successfully uploaded and installed.

    ![Plug-in uploaded](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1667819951/p50464.png)

    If you no longer need the plug-in, find it on the Custom Plug-ins tab and click **Remove** in the Actions column. For more information, see [Install and remove a built-in plug-in](/intl.en-US/Elasticsearch Instances Management/Plug-ins/Built-in plug-ins/Install and remove a built-in plug-in.md).

    **Note:**

    -   If an error occurs when you upload or install a custom plug-in, you can follow the instructions provided in [Installation errors of a custom plug-in](/intl.en-US/FAQ/Installation errors of a custom plug-in.md) to locate and resolve the issue.
    -   Plug-ins are not automatically updated with Elasticsearch. To update a plug-in, you must manually upload a new version of the plug-in.
    -   Custom plug-ins cannot access external networks.

