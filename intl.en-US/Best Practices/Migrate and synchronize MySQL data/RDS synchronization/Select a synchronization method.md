# Select a synchronization method

If you encounter slow queries when you use an ApsaraDB RDS database, you can synchronize data from the database to an Alibaba Cloud Elasticsearch cluster for data queries and analytics. Alibaba Cloud Elasticsearch is a Lucene-based, distributed search and analytics engine. It allows you to store, query, and analyze large amounts of datasets in near real time. You can use Data Transmission Service \(DTS\), Logstash, DataWorks, or Canal to synchronize data from an ApsaraDB RDS database to an Alibaba Cloud Elasticsearch cluster. This topic describes the use scenarios of each method. You can select an appropriate method based on your business requirements.

|Method|Description|Use scenario|Usage note|References|
|------|-----------|------------|----------|----------|
|Use DTS to synchronize data in real time|DTS uses binary logs to synchronize data. You can use DTS to synchronize data within milliseconds, without affecting the source database.|You require a high real-time performance for data synchronization.|-   DTS uses the read and write resources of the source database and destination cluster during data initialization. This may increase the loads of the database and cluster.
-   You can customize mappings for indexes. However, you must make sure that the fields defined in the mappings are the same as those in the source database.
-   You must purchase a data synchronization instance in the DTS console. For more information about how to purchase such an instance, see [Purchase a DTS instance](). For more information about the pricing of DTS, see [Pricing]().

|[Use DTS to dynamically synchronize MySQL data to an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Best Practices/Migrate and synchronize MySQL data/RDS synchronization/Use DTS to dynamically synchronize MySQL data to an Alibaba Cloud Elasticsearch cluster.md)|
|Use the logstash-input-jdbc plug-in to synchronize data|You can use the logstash-input-jdbc plug-in to query the data in an ApsaraDB RDS database and migrate the data to an Elasticsearch cluster. During data synchronization, the plug-in uses a round-robin method to identify the latest inserted or updated data in the database on a regular basis. Then, it queries all identified data at a time and migrates the data to an Elasticsearch cluster. The logstash-input-jdbc plug-in provides lower real-time performance than DTS. Data is synchronized within seconds.|-   You want to synchronize full data and can accept a latency of a few seconds.
-   You want to query specific data at a time and synchronize the data.

|-   Before you use this method, upload an SQL JDBC driver that is compatible with the version of the ApsaraDB RDS database.
-   You must add the IP addresses of the nodes in your Logstash cluster to the whitelist of your ApsaraDB RDS instance.
-   Your Logstash cluster and ApsaraDB RDS instance must reside in the same zone. This avoids inconsistent timestamps during data synchronization.
-   You must make sure that the `_id` field in your Elasticsearch cluster is the same as the `id` field in the ApsaraDB RDS database.
-   When you insert or update data in your ApsaraDB RDS database, make sure that the related record contains a field that indicates the insertion or update time.

|None|
|Use DataWorks to synchronize offline data|DataWorks is a comprehensive service that provides modules such as Data Integration, DataStudio, and Data Quality. You can use DataWorks to import and store structured data, convert and develop the data, and then synchronize the processed data to Elasticsearch clusters or other data systems.|-   You want to synchronize offline big data. DataWorks can collect offline data at a minimum interval of 5 minutes.
-   You want to customize query statements, perform joint queries on multiple tables, and then synchronize data.

|-   You must activate the DataWorks service.
-   If a high transmission speed is required or the environment is complex, you must customize resource groups.
-   You must add the IP addresses of the resource groups to the whitelist of your ApsaraDB RDS instance.

|[Use DataWorks to synchronize MySQL data to Elasticsearch](/intl.en-US/Best Practices/Migrate and synchronize MySQL data/RDS synchronization/Use DataWorks to synchronize MySQL data to Elasticsearch.md)|
|Use Canal to synchronize MySQL data|You can use binary logs to synchronize and subscribe to data in real time.|You require a high real-time performance for data synchronization.|-   You must build a Canal environment on an Elastic Compute Service \(ECS\) instance. However, this increases the costs of data synchronization.
-   Canal V1.1.4 cannot be used to synchronize data to an Elasticsearch V7.X cluster. We recommend that you use Logstash or DTS to synchronize MySQL data to an Elasticsearch V7.X cluster.
-   You can customize mappings for indexes. However, you must make sure that the fields defined in the mappings are the same as those in the source database.

|[Use Canal to synchronize MySQL data to Alibaba Cloud Elasticsearch](/intl.en-US/Best Practices/Migrate and synchronize MySQL data/RDS synchronization/Use Canal to synchronize MySQL data to Alibaba Cloud Elasticsearch.md)|

