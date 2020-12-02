---
keyword: [Logstash third-party libraries, Logstash mysql driver file]
---

# Configure third-party files

You can use the Third-party Libraries function to upload driver files to the configuration files of Alibaba Cloud Logstash. The Third-party Libraries function also allows you to manage the uploaded third-party files.

1.  Log on to the [Alibaba Cloud Elasticsearch console](https://elasticsearch.console.aliyun.com/#/home).

2.  In the top navigation bar, select the region where your cluster resides.

3.  In the left-side navigation pane, click **Logstash Clusters**. On the page that appears, find the target cluster and click its ID in the **Cluster ID/Name** column.

4.  In the left-side navigation pane of the page that appears, click **Cluster Configuration**.

5.  In the **Third-party Libraries** section, click **Manage** next to **Manage**.

    ![Manage](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1542986061/p69310.png)

6.  In the lower part of the **Modify Configuration** pane, click **Configure**.

7.  Click **Upload** and select the files that you want to upload.

    Alibaba Cloud Logstash supports the following [mysql-connector-java](https://mvnrepository.com/artifact/mysql/mysql-connector-java) driver files. You can select the file based on the MySQL version that you use.

    -   [mysql-connector-java-5.1.27.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.27/mysql-connector-java-5.1.27.jar)
    -   [mysql-connector-java-5.1.35.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.35/mysql-connector-java-5.1.35.jar)
    -   [mysql-connector-java-5.1.39-bin.jar](http://static.runoob.com/download/mysql-connector-java-5.1.39-bin.jar)
    -   [mysql-connector-java-5.1.39.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.39/mysql-connector-java-5.1.39.jar)
    -   [mysql-connector-java-5.1.43.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.43/mysql-connector-java-5.1.43.jar)
    -   [mysql-connector-java-5.1.47.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.47/mysql-connector-java-5.1.47.jar)
    -   [mysql-connector-java-5.1.48.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.48/mysql-connector-java-5.1.48.jar)
    -   [mysql-connector-java-5.1.9.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.9/mysql-connector-java-5.1.9.jar)
    -   [mysql-connector-java-6.0.2.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/6.0.2/mysql-connector-java-6.0.2.jar)
    -   [mysql-connector-java-6.0.6.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/6.0.6/mysql-connector-java-6.0.6.jar)
    -   [mysql-connector-java-8.0.11.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.11/mysql-connector-java-8.0.11.jar)
    -   [mysql-connector-java-8.0.17.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.17/mysql-connector-java-8.0.17.jar)
    -   [mysql-connector-java-8.0.18.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.18/mysql-connector-java-8.0.18.jar)
    You can upload multiple driver files at a time. The file names must end with .jar and can contain up to 100 characters. The system checks file names and MD5 checksum values before files are uploaded. If the check fails, an error message that indicates that the files cannot be uploaded is displayed.

    **Warning:** If you upload third-party files for a cluster, the cluster modification is automatically triggered, which may affect the services that are running in the cluster. Proceed with caution.

8.  Click **Save**.

    After the files are uploaded, the **Cluster Configuration** page is displayed. When the cluster modification is complete, the process for configuring third-party files is finished.

9.  Click **Manage** next to **Upload** to view the information about uploaded third-party files in the **Modify Configuration** pane.

    The information about uploaded third-party files contains **File** and **Path**.

    ![Modify Configuration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1542986061/p69324.png)

    **Note:**

    To improve security for a JDBC driver that you use to configure pipelines, you must add `allowLoadLocalInfile=false&autoDeserialize=false` at the end of the `jdbc_connection_string` parameter, for example, `jdbc_connection_string => "jdbc:mysql://xxx.drds.aliyuncs.com:3306/test-database? allowLoadLocalInfile=false&autoDeserialize=false"`. Otherwise, an error message that indicates the check fails is displayed.

    To delete a file that you no longer use, go to the **Modify Configuration** pane, click **Configure** in the lower part, find the file that you want to delete, and then click the **X** icon to delete the file.


