# Use user-created Filebeat to collect MySQL logs

If you want to view and analyze MySQL logs such as slow logs and error logs, you can use Filebeat to collect MySQL logs. Filebeat then sends the logs to Alibaba Cloud Elasticsearch. You can query, analyze, and present these logs in the Kibana console in a visualized manner. This topic describes how to perform the detailed procedure.

## Procedure

1.  [Preparations](#section_pe3_0w7_6wo)

    Create an Alibaba Cloud Elasticsearch cluster and an Elastic Compute Service \(ECS\) instance. The Elasticsearch cluster is used to receive the MySQL logs that are collected by Filebeat. It also provides the Kibana console to query, analyze, and present these logs in a visualized manner. The ECS instance is used to install MySQL and Filebeat.

2.  [Step 1: Install and configure MySQL](#section_hyw_06v_cxp)

    Install MySQL and configure error log files and slow query log files in the MySQL configuration file. Then, Filebeat can collect your desired logs.

3.  [Step 2: Install and configure Filebeat](#section_rmz_4e2_ttp)

    Install Filebeat. Filebeat is used to collect MySQL logs and send the logs to your Elasticsearch cluster. You must enable the MySQL module in Filebeat and specify the URLs that are used to access your Elasticsearch cluster and the Kibana console of the cluster in the Filebeat configuration file.

4.  [Step 3: Use the Kibana dashboard to present MySQL logs](#section_r8e_tyr_2tm)

    Perform a query test and present the error logs and slow query logs that you want to view and analyze on the dashboard of the Kibana console.


## Preparations

-   Create an Elasticsearch cluster.

    An Elasticsearch V6.7.0 cluster of the Standard Edition is used in this topic. For more information, see [Create an Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Elasticsearch cluster.md).

-   Create an Alibaba Cloud ECS instance.

    An ECS instance that runs CentOS is used in this topic. For more information, see [Create an instance by using the provided wizard](/intl.en-US/Instance/Create an instance/Create an instance by using the provided wizard.md).


## Step 1: Install and configure MySQL

1.  Connect to the ECS instance.

    For more information, see [Connect to a Linux instance by using VNC](/intl.en-US/Instance/Connect to instances/Connect to Linux instances/Connect to a Linux instance by using VNC.md).

2.  Download and install the MySQL source.

    ```
    wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
    rpm -ivh mysql-community-release-el7-5.noarch.rpm
    ```

3.  Install MySQL.

    ```
    yum install mysql-server
    ```

4.  Start MySQL and check its status.

    ```
    systemctl start mysqld
    systemctl status mysqld
    ```

5.  Configure error log files and slow query log files in the my.cnf file.

    **Note:** By default, the configuration of log files in MySQL is disabled. You must manually enable the log file configuration. You can also enable temporary slow logs by running a MySQL command.

    1.  Open the my.cnf file.

        ```
        vim /etc/my.cnf
        ```

    2.  Configure log files.

        ```
        [mysqld]
        log_queries_not_using_indexes = 1
        slow_query_log=on
        slow_query_log_file=/var/log/mysql/slow-mysql-query.log
        long_query_time=0
        
        [mysqld_safe]
        log-error=/var/log/mysql/mysqld.log
        ```

        |Parameter|Description|
        |---------|-----------|
        |`log_queries_not_using_indexes`|Specifies whether to record a query for which no indexes are specified as a slow query log. 1: indicates that the system records a query for which no indexes are specified as a slow query log. 0: indicates that the system does not record a query for which no indexes are specified as a slow query log.|
        |`slow_query_log`|Specifies whether to enable slow query logs. on: indicates that slow query logs are enabled. off: indicates that slow query logs are disabled.|
        |`slow_query_log_file`|Specifies the storage path of slow query logs.|
        |`long_query_time`|Specifies the time threshold used to define a slow query log. Unit: seconds. When the query time exceeds the threshold, the MySQL database writes the query into the file that is specified by `slow_query_log_file`.**Note:** For the convenience of the test, the value of this parameter is set to 0. You can specify this parameter based on your business requirements. |

    3.  Create log files.

        ```
        mkdir /var/log/mysql
        touch /var/log/mysql/mysqld.log
        touch /var/log/mysql/slow-mysql-query.log
        ```

        **Note:** MySQL does not automatically create log files. You must manually create the log files.

    4.  Grant read and write permissions on the log files to all users.

        ```
        chmod 777 /var/log/mysql/slow-mysql-query.log /var/log/mysql/mysqld.log
        ```


## Step 2: Install and configure Filebeat

1.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the Visualize and Explore Data section, click **Logs**.

3.  On the page that appears, click **View setup instructions**.

4.  On the **Add Data to Kibana** page, click **MySQL logs**.

5.  In the Getting Started section, click the **RPM** tab.

    **Note:** The Linux operating system is used in this topic. Therefore, **RPM** is selected. You can select an appropriate installation method based on your operating system.

6.  Install Filebeat on your ECS instance as prompted.

7.  Modify the configuration of the MySQL module and specify the files of the error logs and slow logs that you want to collect.

    1.  Enable the MySQL module.

        ```
        sudo filebeat modules enable mysql
        ```

    2.  Open the mysql.yml file.

        ```
        vim /etc/filebeat/modules.d/mysql.yml
        ```

    3.  Modify the configuration of the MySQL module.

        ![Modify the configuration of the MySQL module](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2505742061/p120545.png)

        ```
        - module: mysql
          # Error logs
          error:
            enabled: true
            var.paths: ["/var/log/mysql/mysqld.log"]
            # Set custom paths for the log files. If left empty,
            # Filebeat will choose the paths depending on your OS.
            #var.paths:
        
          # Slow logs
          slowlog:
            enabled: true
            var.paths: ["/var/log/mysql/slow-mysql-query.log"]
            # Set custom paths for the log files. If left empty,
            # Filebeat will choose the paths depending on your OS.
            #var.paths:
        ```

        |Parameter|Description|
        |---------|-----------|
        |`enabled`|Set this parameter to `true`.|
        |`var.paths`|Set this parameter to the path of the log file. The path must be the same as the path that is specified in the MySQL configuration file. For more information, see [Step 1: Install and configure MySQL](#section_hyw_06v_cxp).|

8.  Configure the filebeat.yml file.

    1.  Open the filebeat.yml file.

        ```
        vim /etc/filebeat/filebeat.yml
        ```

    2.  Modify the configuration of Filebeat modules.

        ![Configuration of Filebeat modules](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2505742061/p118153.png)

        ```
        filebeat.config.modules:
          # Glob pattern for configuration loading
          path: /etc/filebeat/modules.d/mysql.yml
        
          # Set to true to enable config reloading
          reload.enabled: true
        
          # Period on which files under path should be checked for changes
          reload.period: 1s
        ```

    3.  Modify the configuration of Kibana.

        ![Modify the configuration of Kibana](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2505742061/p118154.png)

        ```
        setup.kibana:
        host: "https://es-cn-0pp1jxvcl000*****.kibana.elasticsearch.aliyuncs.com:5601"
        ```

        `host`: the URL that is used to access the Kibana console. You can obtain the URL on the Kibana configuration page. For more information, see [View the public endpoint of the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Configure a whitelist for access to the Kibana console over the Internet or an internal network.md). Specify the URL in the format of `<Public endpoint of the Kibana console>:5601`.

    4.  Modify the configuration of the Elasticsearch cluster.

        ![Modify the configuration of the Elasticsearch cluster](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2505742061/p118155.png)

        ```
        output.elasticsearch:
          # Array of hosts to connect to.
          hosts: ["es-cn-0pp1jxvcl000*****.elasticsearch.aliyuncs.com:9200"]
          # Optional protocol and basic auth credentials.
          #protocol: "https"
          username: "elastic"
          password: "<your_password>"
        ```

        |Parameter|Description|
        |---------|-----------|
        |`hosts`|The URL that is used to access your Elasticsearch cluster. Specify the URL in the format of `<Internal or public endpoint of the Elasticsearch cluster>:9200`. You can obtain the internal or public endpoint on the Basic Information page of the cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md). **Note:** If the ECS instance and Elasticsearch cluster reside in the same Virtual Private Cloud \(VPC\), use the internal endpoint. Otherwise, use the public endpoint. If you use the public endpoint to access the Elasticsearch cluster, you must configure a whitelist for access to the Elasticsearch cluster over the Internet. For more information, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md). |
        |`username`|The username that is used to access the Elasticsearch cluster. Default value: elastic.|
        |`password`|The password that corresponds to the elastic username. The password is specified when you create your Elasticsearch cluster. If you forget the password, you can reset it. For more information about the precautions and procedures for resetting a password, see [Reset the access password for an Elasticsearch cluster](/intl.en-US/Elasticsearch Instances Management/Security/Reset the access password for an Elasticsearch cluster.md).|

9.  Run the following command to start Filebeat.

    ```
    sudo filebeat setup
    sudo service filebeat start
    ```


## Step 3: Use the Kibana dashboard to present MySQL logs

1.  Restart MySQL in the ECS instance and query logs for tests.

    Run the following command to restart MySQL:

    ```
    systemctl restart mysqld
    ```

2.  View the queried logs.

    The following figures show the queried logs.

    ![Slow logs](../images/p120563.png "Slow logs")

    ![Error logs](../images/p120564.png "Error logs")

3.  Log on to the Kibana console of your Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

4.  In the left-side navigation pane, click **Dashboard**.

5.  On the **Dashboards** page, click **\[Filebeat MySQL\] Overview**.

6.  Select a time range in the upper-right corner and view the logs in the time range.

    ![View the logs](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3505742061/p120552.png)


