# 通过ES-Hadoop实现Spark读写阿里云Elasticsearch数据

Spark是一种通用的大数据计算框架，拥有Hadoop MapReduce所具有的计算优点，能够通过内存缓存数据为大型数据集提供快速的迭代功能。与MapReduce相比，减少了中间数据读取磁盘的过程，进而提高了处理能力。本文介绍如何通过ES-Hadoop实现Hadoop的Spark服务读写阿里云Elasticsearch数据。

## 准备工作

1.  创建阿里云Elasticsearch实例，并开启自动创建索引功能。

    具体操作步骤请参见[t134282.md\#](/intl.zh-CN/Elasticsearch/管理实例/创建阿里云Elasticsearch实例.md)和[开启自动创建索引](/intl.zh-CN/Elasticsearch/快速访问与配置.md)。本文以6.7.0版本的实例为例。

    **说明：** 在生产环境中，建议关闭自动创建索引功能，提前创建好索引和Mapping。由于本文仅用于测试，因此开启了自动创建索引功能。

2.  创建与Elasticsearch实例在同一专有网络下的E-MapReduce（以下简称EMR）实例。

    实例配置如下：

    -   产品版本：EMR-3.29.0
    -   必选服务：Spark\(2.4.5\)，其他服务保持默认
    具体操作步骤，请参见[创建集群](/intl.zh-CN/快速入门/创建集群.md)。

    **说明：** Elasticsearch实例的私网访问白名单默认为0.0.0.0/0，您可在安全配置页面查看，如果未使用默认配置，您还需要在白名单中加入EMR集群的内网IP地址：

    -   请参见[查看集群列表与详情](/intl.zh-CN/集群管理/集群配置/查看集群列表与详情.md)，获取EMR集群的内网IP地址。
    -   请参见[配置Elasticsearch公网或私网访问白名单](/intl.zh-CN/Elasticsearch/安全配置/配置ES公网或私网访问白名单.md)，配置Elasticsearch实例的VPC私网访问白名单。
3.  准备Java环境，要求JDK版本为8.0及以上。


## 编写并运行Spark任务

1.  准备测试数据。

    1.  登录[E-MapReduce控制台](https://emr.console.aliyun.com/)，获取Master节点的IP地址，并通过SSH登录对应的ECS机器。

        具体操作步骤，请参见[登录集群](/intl.zh-CN/集群管理/集群运维/登录集群.md)。

    2.  将测试数据写入文件中。

        本文使用的JSON数据示例如下，将该数据保存在http\_log.txt文件中。

        ```
        {"id": 1, "name": "zhangsan", "birth": "1990-01-01", "addr": "No.969, wenyixi Rd, yuhang, hangzhou"}
        {"id": 2, "name": "lisi", "birth": "1991-01-01", "addr": "No.556, xixi Rd, xihu, hangzhou"}
        {"id": 3, "name": "wangwu", "birth": "1992-01-01", "addr": "No.699 wangshang Rd, binjiang, hangzhou"}
        ```

    3.  执行以下命令，将测试数据上传至EMR Master节点的tmp/hadoop-es文件中。

        ```
        hadoop fs -put http_log.txt /tmp/hadoop-es
        ```

2.  配置pom依赖。

    创建Java Maven工程，并将如下的pom依赖添加到Java工程的pom.xml文件中。

    ```
    <dependencies>
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-core_2.12</artifactId>
            <version>2.4.5</version>
        </dependency>
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-sql_2.11</artifactId>
            <version>2.4.5</version>
        </dependency>
        <dependency>
            <groupId>org.elasticsearch</groupId>
            <artifactId>elasticsearch-spark-20_2.11</artifactId>
            <version>6.7.0</version>
        </dependency>
    </dependencies>
    ```

    **说明：** 请确保pom依赖中版本与云服务对应版本保持一致，例如elasticsearch-spark-20\_2.11版本与阿里云Elasticsearch版本一致；spark-core\_2.12与HDFS版本一致。

3.  编写示例代码。

    1.  写数据

        以下示例代码用来将测试数据写入Elasticsearch的company索引中。

        ```
        import java.util.Map;
        import java.util.concurrent.atomic.AtomicInteger;
        import org.apache.spark.SparkConf;
        import org.apache.spark.SparkContext;
        import org.apache.spark.api.java.JavaRDD;
        import org.apache.spark.api.java.function.Function;
        import org.apache.spark.sql.Row;
        import org.apache.spark.sql.SparkSession;
        
        import org.elasticsearch.spark.rdd.api.java.JavaEsSpark;
        import org.spark_project.guava.collect.ImmutableMap;
        public class SparkWriteEs {
            public static void main(String[] args) {
                SparkConf conf = new SparkConf();
                conf.setAppName("Es-write");
                conf.set("es.nodes", "es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com");
                conf.set("es.net.http.auth.user", "elastic");
                conf.set("es.net.http.auth.pass", "xxxxxx");
                conf.set("es.nodes.wan.only", "true");
                conf.set("es.nodes.discovery","false");
                conf.set("es.input.use.sliced.partitions","false");
                SparkSession ss = new SparkSession(new SparkContext(conf));
                final AtomicInteger employeesNo = new AtomicInteger(0);
                //以下的/tmp/hadoop-es/http_log.txt需要替换为您测试数据的路径。
                JavaRDD<Map<Object, ?>> javaRDD = ss.read().text("/tmp/hadoop-es/http_log.txt")
                        .javaRDD().map((Function<Row, Map<Object, ?>>) row -> ImmutableMap.of("employees"   employeesNo.getAndAdd(1), row.mkString()));
                JavaEsSpark.saveToEs(javaRDD, "company/_doc");
            }
        }
        ```

    2.  读数据

        以下示例代码用来读取上一步写入Elasticsearch的数据，并进行打印。

        ```
        import org.apache.spark.SparkConf;
        import org.apache.spark.api.java.JavaPairRDD;
        import org.apache.spark.api.java.JavaSparkContext;
        import org.elasticsearch.spark.rdd.api.java.JavaEsSpark;
        import  java.util.Map;
        
        public class ReadES {
        
            public static void main(String[] args) {
        
                SparkConf  conf = new SparkConf().setAppName("readEs").setMaster("local[*]")
                        .set("es.nodes", "es-cn-n6w1o1x0w001c****.elasticsearch.aliyuncs.com")
                        .set("es.port", "9200")
                        .set("es.net.http.auth.user", "elastic")
                        .set("es.net.http.auth.pass", "xxxxxx")
                        .set("es.nodes.wan.only", "true")
                        .set("es.nodes.discovery","false")
                        .set("es.input.use.sliced.partitions","false")
                        .set("es.resource", "company/_doc")
                        .set("es.scroll.size","500");
        
                JavaSparkContext sc = new JavaSparkContext(conf);
        
                JavaPairRDD<String, Map<String, Object>> rdd = JavaEsSpark.esRDD(sc);
        
                for ( Map<String, Object> item : rdd.values().collect()) {
                    System.out.println(item);
                }
        
                sc.stop();
            }
        
        }
        ```

    |参数|默认值|说明|
    |--|---|--|
    |es.nodes|localhost|指定阿里云Elasticsearch实例的访问地址，建议使用内网地址，可在实例的基本信息页面查看。更多信息，请参见[查看实例的基本信息](/intl.zh-CN/Elasticsearch/管理实例/查看实例的基本信息.md)。|
    |es.port|9200|Elasticsearch实例的访问端口号。|
    |es.net.http.auth.user|elastic|Elasticsearch实例的访问用户名。**说明：** 如果程序中指定elastic账号访问Elasticsearch服务，后续在修改elastic账号对应密码后需要一些时间来生效，在密码生效期间会影响服务访问，因此不建议通过elastic来访问。建议在Kibana控制台中创建一个符合预期的Role角色用户进行访问，详情请参见[创建角色](/intl.zh-CN/访问控制/Kibana角色管理/创建角色.md)和[创建用户](/intl.zh-CN/访问控制/Kibana角色管理/创建用户.md)。 |
    |es.net.http.auth.pass|/|对应用户的密码，在创建实例时指定。如果忘记可进行重置，具体操作步骤，请参见[重置实例访问密码](/intl.zh-CN/Elasticsearch/安全配置/重置实例访问密码.md)。|
    |es.nodes.wan.only|false|开启Elasticsearch集群在云上使用虚拟IP进行连接，是否进行节点嗅探：    -   true：设置
    -   false：不设置 |
    |es.nodes.discovery|true|是否禁用节点发现：    -   true：禁用
    -   false：不禁用
**说明：** 使用阿里云Elasticsearch，必须将此参数设置为false。 |
    |es.input.use.sliced.partitions|true|是否使用slice分区：    -   true：使用。设置为true，可能会导致索引在预读阶段的时间明显变长，有时会远远超出查询数据所耗费的时间。建议设置为false，以提高查询效率。
    -   false：不使用。 |
    |es.index.auto.create|true|通过Hadoop组件向Elasticsearch集群写入数据，是否自动创建不存在的index：    -   true：自动创建
    -   false：不会自动创建 |
    |es.resource|/|指定要读写的index和type。|
    |es.mapping.names|/|表字段与Elasticsearch的索引字段名映射。|

    更多的ES-Hadoop配置项说明，请参见[官方配置说明](https://www.elastic.co/guide/en/elasticsearch/hadoop/current/configuration.html?spm=a2c4g.11186623.2.28.2ce94609aDHRug)。

4.  将代码打成Jar包，上传至EMR客户端机器（例如Gateway或EMR集群主节点）。

5.  在EMR客户端机器上，运行如下命令执行Spark程序：

    -   写数据

        ```
        cd /usr/lib/spark-current
        ./bin/spark-submit  --master yarn --executor-cores 1 --class "SparkWriteEs" /root/spark_es.jar
        ```

        **说明：** /root/spark\_es.jar需要替换为您Jar包上传的路径。

    -   读数据

        ```
        cd /usr/lib/spark-current
        ./bin/spark-submit  --master yarn --executor-cores 1 --class "ReadES"  /root/spark_es.jar
        ```

        读数据成功后，打印结果如下。

        ![打印成功结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2167094061/p180140.png)


## 验证结果

1.  登录对应阿里云Elasticsearch实例的Kibana控制台。

    具体操作步骤请参见[登录Kibana控制台](/intl.zh-CN/Elasticsearch/可视化控制/Kibana/登录Kibana控制台.md)。

2.  在左侧导航栏，单击**Dev Tools**。

3.  在**Console**中，执行以下命令，查看通过Spark任务写入的数据。

    ```
    GET company/_search
    {
      "query": {
        "match_all": {}
      }
    }
    ```

    查询成功后，返回结果如下。

    ![查询成功结果](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2167094061/p180149.png)


## 总结

本文以阿里云Elasticsearch和EMR为例，介绍了如何通过ES-Hadoop，实现Spark读写阿里云Elasticsearch数据。与其他EMR组件相比，ES-Hadoop与Spark的集成，不仅包括RDD，还包括Spark Streaming、scale、DataSet与Spark SQL等，您可以根据需求进行配置。详细信息，请参见[Apache Spark support](https://www.elastic.co/guide/en/elasticsearch/hadoop/master/spark.html)。

