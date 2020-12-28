# Logstash configuration files

This topic describes pipeline configuration files of Alibaba Cloud Logstash.

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

Each part can contain one or more plug-ins. If you specify multiple filter plug-ins, Logstash applies the plug-ins in the order that they appear in the configuration file.

**Note:**

-   If the configuration file contains a parameter similar to `last_run_metadata_path`, the file path must be provided by Logstash. The Logstash backend provides the /ssd/1/ls-cn-xxxxxxx/logstash/data/ path for test purposes. The system does not delete the data stored in this path. Make sure that your disk has sufficient storage space when you use this path.
-   For security purposes, if you use a JDBC driver to configure a pipeline, you must add `allowLoadLocalInfile=false&autoDeserialize=false` at the end of the `jdbc_connection_string` parameter, such as `jdbc_connection_string => "jdbc:mysql://xxx.drds.aliyuncs.com:3306/test-database?allowLoadLocalInfile=false&autoDeserialize=false"`. Otherwise, the system displays an error message that indicates a check failure.

## Plug-in configuration

The configuration of a plug-in consists of the plug-in name and a collection of plug-in properties. In the following configuration, the input part defines two beats plug-ins, and each plug-in contains the `port` and `host` properties.

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

The configurations supported by different plug-ins vary based on the plug-in types. For more information, see [Input plugins](https://www.elastic.co/guide/en/logstash/7.4/input-plugins.html), [Output plugins](https://www.elastic.co/guide/en/logstash/7.4/output-plugins.html), [Filter plugins](https://www.elastic.co/guide/en/logstash/7.4/filter-plugins.html), and [Codec plugins](https://www.elastic.co/guide/en/logstash/7.4/codec-plugins.html).

## Value types

When you configure a plug-in, you can set the following types of values:

-   Array

    This type is not recommended. We recommend that you use a standard type, such as string. You can define the `:list => true` property to facilitate type checks. In addition, you must process hash tables or lists of mixed types that do not require type checks. Example:

    ```
    users => [ {id => 1, name => bob}, {id => 2, name => jane} ]
    ```

-   List

    The list itself is not a type, but its attributes feature type characteristics. You can enter multiple values for type checks. If you specify `:list => true` when you declare a parameter, the plug-in enables list checks. Example:

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

    A field of the byte type is a string field that represents a number of valid bytes. This is a convenient way to declare specific sizes in plug-in options. The byte type supports SI \(k MGTPEZY\) and binary \(Ki Mi Gi Ti Pi Ei Zi Yi\). SI units are in base-1000 and binary units are in base-1024. The field is not case-sensitive and allows a space between the value and the unit. If no units are specified, an integer string indicates the number of bytes. Example:

    ```
     my_bytes => "1113"   # 1113 bytes
     my_bytes => "10MiB"  # 10485760 bytes
     my_bytes => "100kib" # 102400 bytes
     my_bytes => "180 mb" # 180000000 bytes
    ```

-   Codec

    A codec is a type of data that is encoded or decoded. It can be used in both the input and output plug-ins.

    An input codec decodes data before the data enters the input plug-in. An output codec encodes data before the data leaves the output plug-in. If you use an input or output codec, you do not need to specify filter plug-ins for your Logstash pipeline.

    For more information about available codecs, see [Codec plugins](https://www.elastic.co/guide/en/logstash/7.4/codec-plugins.html).

    Example:

    ```
    codec => "json"
    ```

-   Hash

    A value of the hash type is a collection of key-value pairs, such as `"field1" => "value1"`.

    **Note:** Multiple key-value pairs are separated by spaces instead of commas \(,\).

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

    A URL ranges from a full URL to a simple identifier, such as foobar. If a URL contains a password, such as `http://user:paas@example.net`, the password is not recorded or displayed.

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

    By default, Logstash does not enable escape sequences. If you want to use escape sequences in quoted strings, add `config.support_escapes: true` in logstash.yml. If config.support\_escapes is set to `true`, the quoted strings \(double- and single-precision\) are converted.

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


