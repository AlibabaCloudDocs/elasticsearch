# Logstash配置文件说明

本文提供阿里云Logstash管道配置文件的详细说明。

您可以通过配置文件管理方式，修改Logstash的配置文件，配置管道，完成数据传输。详细信息，请参见[通过配置文件管理管道](/cn.zh-CN/Logstash/管道任务管理/通过配置文件管理管道.md)。

## 配置文件结构

Logstash配置文件对每种类型的插件都提供了一个单独的配置部分，用于管道事件处理。

```
input {
  ...
}

filter {
  ...
}

output {
  ...
}
```

每个配置部分可以包含一个或多个插件。例如，指定多个filter插件，Logstash会按照它们在配置文件中出现的顺序，进行处理。

**说明：**

-   如果配置中有类似`last_run_metadata_path`的参数，需要Logstash服务提供文件路径。目前阿里云Logstash后端开放了/ssd/1/<Logstash实例ID\>/logstash/data/路径供您测试使用，且该目录下的数据不会删除，因此在使用时，请确保磁盘有充足的使用空间。
-   为了提升安全性，如果在配置管道时使用了JDBC驱动，需要在`jdbc_connection_string`参数后面添加`allowLoadLocalInfile=false&autoDeserialize=false`，否则当您在添加Logstash配置文件的时候，调度系统会抛出校验失败的提示，例如`jdbc_connection_string => "jdbc:mysql://xxx.drds.aliyuncs.com:3306/<数据库名称>?allowLoadLocalInfile=false&autoDeserialize=false"`。

## 插件配置

插件的配置包括插件名称，以及名称中包含的一组插件配置属性。例如，以下input部分包含了两个beats插件，每个beats插件中都配置了`port`和`host`属性。

```
input {
  beats {
    port => 5044
    host => "118.11.xx.xx"
  }

  beats {
    port => 514
    host => "192.168.xx.xx"
  }
}
```

插件支持的配置因插件类型而异，各插件的详细信息请参见[输入插件](https://www.elastic.co/guide/en/logstash/7.4/input-plugins.html)、[输出插件](https://www.elastic.co/guide/en/logstash/7.4/output-plugins.html)、[过滤器插件](https://www.elastic.co/guide/en/logstash/7.4/filter-plugins.html)、[编解码器插件](https://www.elastic.co/guide/en/logstash/7.4/codec-plugins.html)。

## 值类型

配置插件时，您可以设置插件的值类型，例如布尔值、列表、哈希等。插件支持以下值类型：

-   数组

    目前不推荐使用此类型，而建议使用标准类型（例如String），使用插件定义`:list => true`属性，以便更好地进行类型检查。同时仍然需要处理不需要类型检查的哈希表，或者混合类型列表，示例如下。

    ```
    users => [ {id => 1, name => bob}, {id => 2, name => jane} ]
    ```

-   列表

    列表本身不具备类型特征，但其所包含的属性具有类型特征。这样就可以键入检查多个值。这里可以通过列表的形式，在声明参数时指定启用检查`:list => true`，示例如下。

    ```
    path => [ "/var/log/messages", "/var/log/*.log" ]
    uris => [ "http://elastic.co", "http://example.net" ]
    ```

    以上示例，将`path`配置为一个列表，该列表中包含2个字符串。`uris`也为一个列表，如果所包含的URL无效，会导致事件处理失败。

-   布尔类型

    布尔类型的值必须为`true`或者`false`，且不需要引号标注，示例如下。

    ```
    ssl_enable => true
    ```

-   字节类型

    字节类型的字段，代表有效字节单位的字符串字段。这是在插件选项中，声明特定大小的便捷方法。字节类型支持SI（k MGTPEZY）和Binary（Ki Mi Gi Ti Pi Ei Zi Yi）。二进制单位为base-1024，SI单位为base-1000。该字段不区分大小写，并且接受值和单位之间的空格。如果未指定单位，则整数字符串表示字节数。示例如下。

    ```
     my_bytes => "1113"   # 1113 bytes
     my_bytes => "10MiB"  # 10485760 bytes
     my_bytes => "100kib" # 102400 bytes
     my_bytes => "180 mb" # 180000000 bytes
    ```

-   编解码器

    编解码器是用于对数据进行编码，或者解码后的目标数据类型，在输入和输出插件中都可以使用。

    输入编解码器，提供了在数据输入之前对其进行解码的功能。输出编解码器，提供了在数据输出之前对其进行编码的功能。使用输入或输出编解码器后，您不需要在Logstash管道中，单独使用过滤器。

    您可以参考官方提供的[编译解释器插件](https://www.elastic.co/guide/en/logstash/7.4/codec-plugins.html)文档，查找可用的编解码器。

    示例如下。

    ```
    codec => "json"
    ```

-   哈希

    哈希格式指定键值对的集合，例如`"field1" => "value1"`。

    **说明：** 多个键值对使用空格进行分隔，而不是逗号。

    示例如下。

    ```
    match => {
      "field1" => "value1"
      "field2" => "value2"
      ...
    }
    # or as a single line. No commas between entries:
    match => { "field1" => "value1" "field2" => "value2" }
    ```

-   数值

    必须是有效的数值（浮点数或整数）。

    示例如下。

    ```
    port => 33
    ```

-   密码

    密码是一个没有记录或打印的单值字符串。

    示例如下。

    ```
    my_password => "password"
    ```

-   URL

    URL可以是任何内容，从完整的URL到简单的标识符（如foobar）。如果URL中包含类似`http://user:paas@example.net`的密码，那么密码部分不会被记录或打印。

    ```
     my_uri => "http://foo:bar@example.net"
    ```

-   路径

    路径是代表有效操作系统路径的字符串。

    示例如下。

    ```
    my_path => "/tmp/logstash"
    ```

-   字符串

    字符串必须是单个字符序列，且必须用双引号或单引号括起来。

-   转义序列

    默认情况下，Logstash不启用转义序列。如果您希望在带引号的字符串使用转义序列，需要在logstash.yml中设置`config.support_escapes: true`。当设置为`true`时，带引号的字符串（双精度和单精度）可以进行一下转换。

    |文本|结果|
    |--|--|
    |\\r|回车（ASCII 13）|
    |\\n|换行（ASCII 10）|
    |\\t|跳格（ASCII 9）|
    |\\\\|反斜杠（ASCII 92）|
    |\\"|双引号（ASCII 34）|
    |\\'|单引号（ASCII 39）|

    示例如下。

    ```
    name => 'It\'s a beautiful day'
    ```


