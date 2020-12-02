# Logstash configuration files

This topic describes pipeline configuration files in Alibaba Cloud Logstash.

You can modify a configuration file to configure the pipeline for specific data transmission tasks in Logstash. For more information, see [Use configuration files to manage pipelines](/intl.en-US/Logstash/Pipeline task management/Use configuration files to manage pipelines.md).

## Configuration file structure

A Logstash configuration file allows you to configure each type of plug-in in a separate part.

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

Each part can contain one or more plug-ins. For example, if you specify multiple filter plug-ins, Logstash applies the plug-ins in the order that they appear in the configuration file.

**Note:**

-   If the configuration file contains a parameter similar to `last_run_metadata_path`, the file path must be provided by Logstash. The Logstash backend provides /ssd/1/ls-cn-xxxxxxx/logstash/data/ for test purposes. Data stored in this path will not be deleted. Make sure that your disk has sufficient storage space when you use this path.
-   If you have used the JDBC driver to configure a pipeline, you must append `allowLoadLocalInfile=false&autoDeserialize=false` following `jdbc_connection_string` to improve security. Otherwise, when you add a Logstash configuration file, the scheduling system reports a verification failure error. Example: `jdbc_connection_string => "jdbc:mysql://xxx.drds.aliyuncs.com:3306/test-database? allowLoadLocalInfile=false&autoDeserialize=false"`.

## Plug-in configuration

The configuration of a plug-in consists of the plug-in name and a set of plug-in properties. For example, the input part in the following configuration defines two file plug-ins, and each plug-in contains the `path` and `type` properties.

```
input {
  file {
    path => "/var/log/messages"
    type => "syslog"
  }

  file {
    path => "/var/log/apache/access.log"
    type => "apache"
  }
}
```

The configurations supported by different plug-ins vary with the plug-in types. For more information, see [Input plugins](https://www.elastic.co/guide/en/logstash/7.4/input-plugins.html), [Output plugins](https://www.elastic.co/guide/en/logstash/7.4/output-plugins.html), [Filter plugins](https://www.elastic.co/guide/en/logstash/7.4/filter-plugins.html), and [Codec plug-ins](https://www.elastic.co/guide/en/logstash/7.4/codec-plugins.html).

## Value types

When you configure a plug-in, you can set the following types of values:

-   Array

    This type is not recommended. We recommend that you use a standard type, such as string. You can define the `:list => true` property for better type checking. Also, you must process hash tables or lists of mixed types that do not require type checking. Example:

    ```
    users => [ {id => 1, name => bob}, {id => 2, name => jane} ]
    ```

-   List

    The list itself is not a type, but its attributes feature type characteristics. You can enter multiple values for type checking. If you specify `:list => true` when you declare a parameter, the plug-in enables list checking. Example:

    ```
    path => [ "/var/log/messages", "/var/log/*.log" ]
    uris => [ "http://elastic.co", "http://example.net" ]
    ```

    In this example, `path` is configured as a list that contains two strings. `uris` is configured as a list of URLs. If one of the URLs is invalid, event processing fails.

-   Boolean

    A value of the Boolean type must be `true` or `false`. The value is not enclosed in quotation marks. Example:

    ```
    ssl_enable => true
    ```

-   Byte

    A byte field is a string field that represents a number of valid bytes. This is a convenient way to declare specific sizes in plug-in options. The byte type supports SI \(k MGTPEZY\) and binary \(Ki Mi Gi Ti Pi Ei Zi Yi\). SI units are in base-1000 and binary units are in base-1024. The field is not case sensitive and accepts a space between the value and the unit. If no units are specified, an integer string indicates the number of bytes. Example:

    ```
     my_bytes => "1113"   # 1113 bytes
     my_bytes => "10MiB"  # 10485760 bytes
     my_bytes => "100kib" # 102400 bytes
     my_bytes => "180 mb" # 180000000 bytes
    ```

-   Codec

    A codec is a type of data that is encoded or decoded. It can be used in both the input and output plug-ins.

    An input codec decodes data before the data enters the input plug-in. An output codec encodes data before the data leaves the output plug-in. If you use an input or output codec, you do not need the filter plug-ins for your Logstash pipeline.

    For more information about available codecs, see [Codec plug-ins](https://www.elastic.co/guide/en/logstash/7.4/codec-plugins.html).

    Example:

    ```
    codec => "json"
    ```

-   Hash

    A value of the hash type is a collection of key-value pairs, for example, `"field1" => "value1"`.

    **Note:** Multiple key-value pairs are separated with spaces instead of commas.

    Example:

    ```
    match => {
      "field1" => "value1"
      "field2" => "value2"
      ...
    }
    # or as a single line. No commas between entries:
    match => { "field1" => "value1" "field2" => "value2" }
    ```

-   Numeric

    A value of the numeric type must be a floating point or an integer.

    Example:

    ```
    port => 33
    ```

-   Password

    A password is a string with a single value that is not recorded or displayed.

    Example:

    ```
    my_password => "password"
    ```

-   URL

    A URL ranges from a full URL to a simple identifier, such as foobar. If a URL contains a password, such as `http://user:paas@example.net`, the password portion is not recorded or displayed.

    ```
     my_uri => "http://foo:bar@example.net"
    ```

-   Path

    A path is a string that represents a valid path in an operating system.

    Example:

    ```
    my_path => "/tmp/logstash" 
    ```

-   String

    A string must be a single sequence of characters enclosed in double quotation marks \("\) or single quotation marks \('\).

-   Escape sequence

    By default, Logstash does not enable escape sequences. If you want to use escape sequences in quoted strings, add `config.support_escapes: true` in logstash.yml. When `true` is used, the quoted strings \(double- and single-precision\) are converted.

    |Text|Result|
    |----|------|
    |\\r|Carriage return \(ASCII 13\)|
    |\\n|Line break \(ASCII 10\)|
    |\\t|Tab \(ASCII 9\)|
    |\\\\|Backslash \(ASCII 92\)|
    |\\"|Double quotation mark \(ASCII 34\)|
    |\\'|Single quotation mark \(ASCII 39\)|

    Example:

    ```
    name => 'It\'s a beautiful day'
    ```


