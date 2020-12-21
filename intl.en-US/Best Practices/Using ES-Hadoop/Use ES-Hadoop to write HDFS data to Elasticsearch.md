# Use ES-Hadoop to write HDFS data to Elasticsearch

ES-Hadoop is a tool developed by open source Elasticsearch. It connects Elasticsearch to Apache Hadoop and enables data transmission between them. ES-Hadoop combines the quick search capability of Elasticsearch and the batch processing capability of Hadoop to achieve interactive data processing. For some complex data analytics tasks, you must run a MapReduce task to read data from the JSON files stored in Hadoop Distributed File System \(HDFS\) and write the data to an Elasticsearch cluster. This topic describes how to use ES-Hadoop to run such a MapReduce task.

## Procedure

1.  [Preparations](#section_gav_ucb_7ax)

    Create an Alibaba Cloud Elasticsearch cluster and an E-MapReduce \(EMR\) cluster in the same virtual private cloud \(VPC\). Then, enable the Auto Indexing feature for the Elasticsearch cluster, and prepare test data and a Java environment.

2.  [Step 1: Upload the ES-Hadoop JAR package to HDFS](#section_sem_ans_igq)

    Download the ES-Hadoop package and upload the package to the HDFS directory on the master node in the EMR cluster.

3.  [Step 2: Configure POM dependencies](#section_iff_jg3_eg4)

    Create a Java Maven project and configure POM dependencies.

4.  [Step 3: Compile code and run a MapReduce task](#section_k9r_c2d_np5)

    Compile the Java code that is used to write data to the Elasticsearch cluster. Compress the code into a JAR package and upload the package to the EMR cluster. Then, run the code in a MapReduce task to write data.

5.  [Step 4: Verify the results](#section_8we_9o7_iv2)

    Log on to the Kibana console of the Elasticsearch cluster. Then, query the data that is written by the MapReduce task.


## Preparations

1.  Create an Alibaba Cloud Elasticsearch cluster and enable the Auto Indexing feature for the cluster.

    For more information, see [Create an Alibaba Cloud Elasticsearch cluster](/intl.en-US/Quick Start/Step 1: Create a cluster/Create an Alibaba Cloud Elasticsearch cluster.md) and [Enable the Auto Indexing feature](/intl.en-US/Quick Start/Step 2: (Optional) Configure a cluster.md). In this topic, an Elasticsearch V6.7.0 cluster is created.

    **Note:** In a production environment, we recommend that you disable the Auto Indexing feature. You must create an index and configure mappings for the index in advance. The Elasticsearch cluster used in this topic is only for tests. Therefore, the Auto Indexing feature is enabled.

2.  Create an EMR cluster that resides in the same VPC as the Elasticsearch cluster.

    EMR cluster configuration:

    -   EMR Version: Select EMR-3.29.0.
    -   Required Services: HDFS \(2.8.5\) is one of the required services. Default settings are retained for other services.
    For more information, see [Create a cluster](/intl.en-US/Quick Start/Create a cluster.md).

    **Note:** By default, 0.0.0.0/0 is specified in the private IP address whitelist of the Elasticsearch cluster. You can view the whitelist configuration on the cluster security configuration page. If the default setting is not used, you must add the private IP address of the EMR cluster to the whitelist.

    -   For more information about how to obtain the private IP address of the EMR cluster, see [View the cluster list and cluster details](/intl.en-US/Cluster Management/Configure clusters/View the cluster list and cluster details.md).
    -   For more information about how to configure the private IP address whitelist of the Elasticsearch cluster, see [Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC](/intl.en-US/Elasticsearch Instances Management/Security/Configure a whitelist to access an Elasticsearch cluster over the Internet or a VPC.md). The IP addresses in the whitelist can be used to access the Elasticsearch cluster over a VPC.
3.  Prepare JSON-formatted test data and write the data to the map.json file. Upload the file to the /tmp/hadoop-es directory of HDFS.

    The following test data is used in this topic:

    ```
    {"id": 1, "name": "zhangsan", "birth": "1990-01-01", "addr": "No.969, wenyixi Rd, yuhang, hangzhou"}
    {"id": 2, "name": "lisi", "birth": "1991-01-01", "addr": "No.556, xixi Rd, xihu, hangzhou"}
    {"id": 3, "name": "wangwu", "birth": "1992-01-01", "addr": "No.699 wangshang Rd, binjiang, hangzhou"}
    ```

4.  Prepare a Java environment. The JDK version must be 1.8.0 or later.


## Step 1: Upload the ES-Hadoop JAR package to HDFS

1.  Download an [ES-Hadoop package](https://www.elastic.co/cn/downloads/hadoop) that is compatible with the version of the Elasticsearch cluster.

    The elasticsearch-hadoop-6.7.0.zip package is used in this topic.

2.  Log on to the [EMR console](https://emr.console.aliyun.com/) and obtain the IP address of the master node of the EMR cluster. Then, use SSH to log on to the Elastic Compute Service \(ECS\) instance that is indicated by the IP address.

    For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

3.  Upload the elasticsearch-hadoop-6.7.0.zip package to the master node in the EMR cluster. Decompress the package to obtain the elasticsearch-hadoop-6.7.0.jar file.

4.  Create an HDFS directory and upload the elasticsearch-hadoop-6.7.0.jar file to the directory.

    ```
    hadoop fs -mkdir /tmp/hadoop-es
    hadoop fs -put elasticsearch-hadoop-6.7.0/dist/elasticsearch-hadoop-6.7.0.jar /tmp/hadoop-es
    ```


## Step 2: Configure POM dependencies

Create a Java Maven project and add the following POM dependencies to the pom.xml file of the project.

```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.4.1</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                <mainClass>WriteToEsWithMR</mainClass>
                            </transformer>
                        </transformers>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>

<dependencies>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-hdfs</artifactId>
        <version>2.8.5</version>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-mapreduce-client-jobclient</artifactId>
        <version>2.8.5</version>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-common</artifactId>
        <version>2.8.5</version>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-auth</artifactId>
        <version>2.8.5</version>
    </dependency>

    <dependency>
        <groupId>org.elasticsearch</groupId>
        <artifactId>elasticsearch-hadoop-mr</artifactId>
        <version>6.7.0</version>
    </dependency>

    <dependency>
        <groupId>commons-httpclient</groupId>
        <artifactId>commons-httpclient</artifactId>
        <version>3.1</version>
    </dependency>
</dependencies>
```

**Note:** Make sure that the versions of POM dependencies are consistent with those of the related Alibaba Cloud services. For example, the version of elasticsearch-hadoop-mr is consistent with that of Alibaba Cloud Elasticsearch, and the version of hadoop-hdfs is consistent with that of HDFS.

## Step 3: Compile code and run a MapReduce task

1.  Compile code.

    The following code reads data from the JSON files in the /tmp/hadoop-es directory of HDFS. The code also writes each row of data in these JSON files as a document to the Elasticsearch cluster. Data write is finished by EsOutputFormat in the map stage.

    ```
    import java.io.IOException;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.input.TextInputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;
    import org.elasticsearch.hadoop.mr.EsOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;
    
    
    public class WriteToEsWithMR extends Configured implements Tool {
    
        public static class EsMapper extends Mapper<Object, Text, NullWritable, Text> {
            private Text doc = new Text();
    
            @Override
            protected void map(Object key, Text value, Context context) throws IOException, InterruptedException {
                if (value.getLength() > 0) {
                    doc.set(value);
                    System.out.println(value);
                    context.write(NullWritable.get(), doc);
                }
            }
        }
        public int run(String[] args) throws Exception {
            Configuration conf = new Configuration();
            String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    
            conf.setBoolean("mapreduce.map.speculative", false);
            conf.setBoolean("mapreduce.reduce.speculative", false);
            conf.set("es.nodes", "es-cn-4591jumei000u****.elasticsearch.aliyuncs.com");
            conf.set("es.port","9200");
            conf.set("es.net.http.auth.user", "elastic");
            conf.set("es.net.http.auth.pass", "xxxxxx");
            conf.set("es.nodes.wan.only", "true");
            conf.set("es.nodes.discovery","false");
            conf.set("es.input.use.sliced.partitions","false");
            conf.set("es.resource", "maptest/_doc");
            conf.set("es.input.json", "true");
    
            Job job = Job.getInstance(conf);
            job.setInputFormatClass(TextInputFormat.class);
            job.setOutputFormatClass(EsOutputFormat.class);
            job.setMapOutputKeyClass(NullWritable.class);
            job.setMapOutputValueClass(Text.class);
            job.setJarByClass(WriteToEsWithMR.class);
            job.setMapperClass(EsMapper.class);
    
            FileInputFormat.setInputPaths(job, new Path(otherArgs[0]));
    
            return job.waitForCompletion(true) ? 0 : 1;
        }
    
        public static void main(String[] args) throws Exception {
            int ret = ToolRunner.run(new WriteToEsWithMR(), args);
            System.exit(ret);
        }
    }
    ```

    |Parameter|Default value|Description|
    |---------|-------------|-----------|
    |es.nodes|localhost|The endpoint that is used to access the Elasticsearch cluster. We recommend that you use the internal endpoint. You can obtain the internal endpoint on the Basic Information page of the Elasticsearch cluster. For more information, see [View the basic information of a cluster](/intl.en-US/Elasticsearch Instances Management/Manage clusters/View the basic information of a cluster.md).|
    |es.port|9200|The port number that is used to access the Elasticsearch cluster.|
    |es.net.http.auth.user|elastic|The username that is used to access the Elasticsearch cluster.**Note:** If you use the elastic account to access your Elasticsearch cluster and then reset the password of the account, it may require some time for the new password to take effect. During this period, you cannot use the elastic account to access the cluster. Therefore, we recommend that you do not use the elastic account to access an Elasticsearch cluster. You can log on to the Kibana console and create a user with the required role to access an Elasticsearch cluster. For more information, see [Create a role](/intl.en-US/RAM/Manage Kibana role/Create a role.md) and [Create a user](/intl.en-US/RAM/Manage Kibana role/Create a user.md). |
    |es.net.http.auth.pass|/|The password that is used to access the Elasticsearch cluster.|
    |es.nodes.wan.only|false|Specifies whether to enable node sniffing when the Elasticsearch cluster uses a virtual IP address for connections. Valid values:    -   true: indicates that node sniffing is enabled.
    -   false: indicates that node sniff is disabled. |
    |es.nodes.discovery|true|Specifies whether to prohibit the node discovery mechanism. Valid values:    -   true: indicates that the node discovery mechanism is prohibited.
    -   false: indicates that the node discovery mechanism is not prohibited.
**Note:** If you use Alibaba Cloud Elasticsearch, you must set this parameter to false. |
    |es.input.use.sliced.partitions|true|Specifies whether to use partitions. Valid values:    -   true: indicates that partitions are used. In this case, more time may be required for the index read-ahead phase. The time required for this phase may be longer than the time required for data queries. To improve query efficiency, we recommend that you set this parameter to false.
    -   false: indicates that partitions are not used. |
    |es.index.auto.create|true|Specifies whether the system creates an index in the Elasticsearch cluster when you use ES-Hadoop to write data to the cluster. Valid values:    -   true: indicates that the system creates an index in the Elasticsearch cluster.
    -   false: indicates that the system does not create an index in the Elasticsearch cluster. |
    |es.resource|/|The name and type of the index on which data read or write operations are performed.|
    |es.input.json|false|Specifies whether the input data is in the JSON format.|
    |es.mapping.names|/|The mappings between the field names in the table and those in the index of the Elasticsearch cluster.|
    |es.read.metadata|false|Specifies whether to include the document metadata such as **\_id** in the results. To include the document metadata, set the value to true.|

    For more information about the configuration items of ES-Hadoop, see [open source ES-Hadoop configuration](https://www.elastic.co/guide/en/elasticsearch/hadoop/current/configuration.html).

2.  Compress the code into a JAR package and upload it to an EMR client, such as the master node in the EMR cluster or the gateway cluster that is associated with this EMR cluster.

3.  On the EMR client, run the following command to run the MapReduce task:

    ```
    hadoop jar es-mapreduce-1.0-SNAPSHOT.jar /tmp/hadoop-es/map.json
    ```

    **Note:** Replace es-mapreduce-1.0-SNAPSHOT.jar with the name of the uploaded JAR file.


## Step 4: Verify the results

1.  Log on to the Kibana console of the Elasticsearch cluster.

    For more information, see [Log on to the Kibana console](/intl.en-US/Elasticsearch Instances Management/Data visualization/Kibana/Log on to the Kibana console.md).

2.  In the left-side navigation pane, click **Dev Tools**.

3.  On the **Console** tab of the page that appears, run the following command to query the data that is written by the MapReduce task:

    ```
    GET maptest/_search
    {
      "query": {
        "match_all": {}
      }
    }
    ```

    If the command is successfully run, the result shown in the following figure is returned.

    ![Returned result](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1066945061/p173107.png)


## Summary

This topic describes how to use ES-Hadoop to write data to Elasticsearch by running a MapReduce task in an EMR cluster. You can also run a MapReduce task to read data from Elasticsearch. The configurations for data read operations are similar to those for data write operations. For more information, see [Reading data from Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/hadoop/6.8/mapreduce.html#mr-reading) in open source Elasticsearch documentation.

