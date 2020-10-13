# 通过reindex将自建ES数据迁移至阿里云ES

本文介绍通过reindex方式，将ECS内自建的Elasticsearch中的数据迁移至阿里云Elasticsearch（简称ES）中，包括创建索引和迁移数据。

## 注意事项

自建ES使用reindex迁移数据到阿里云ES，仅支持单可用区的阿里云ES实例。如果您使用的是多可用区实例，建议使用如下方案：

-   如果源端数据量比较大，建议使用OSS快照方式。
-   如果需要对源端数据进行过滤，建议使用Losgatsh迁移方案。

## 前提条件

-   自建ES所在的ECS的网络类型必须是专有网络VPC（Virtual Private Cloud），不支持Classiclink方式打通的ECS，且自建ES必须与阿里云ES在同一个VPC下。
-   自建ES所在的ECS的安全组不能限制阿里云ES实例的各节点IP（Kibana控制台可查看各节点的IP），且要开启9200端口。
-   自建ES与阿里云ES实例已经连通。可在执行脚本的机器上使用`curl -XGET http://<host>:9200`进行验证。

    **说明：** 您可以通过任意一台机器执行文档中的脚本，前提是该机器可以同时访问自建ES与阿里云ES集群的9200端口。


## 创建索引

参考自建ES集群中需要迁移的索引配置，提前在阿里云ES集群中创建索引。或者为阿里云ES集群开启自动创建索引功能（不建议）。

以Python为例，使用如下脚本在阿里云ES集群中批量创建自建ES集群中需要迁移的索引，默认新创建的索引副本数为0。

```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
# 文件名：indiceCreate.py
import sys
import base64
import time
import httplib
import json
## 旧集群host
oldClusterHost = "old-cluster.com"
## 旧集群用户名，可为空
oldClusterUserName = "old-username"
## 旧集群密码，可为空
oldClusterPassword = "old-password"
## 新集群host
newClusterHost = "new-cluster.com"
## 新集群用户名，可为空
newClusterUser = "new-username"
## 新集群密码，可为空
newClusterPassword = "new-password"
DEFAULT_REPLICAS = 0
def httpRequest(method, host, endpoint, params="", username="", password=""):
    conn = httplib.HTTPConnection(host)
    headers = {}
    if (username != "") :
        'Hello {name}, your age is {age} !'.format(name = 'Tom', age = '20')
        base64string = base64.encodestring('{username}:{password}'.format(username = username, password = password)).replace('\n', '')
        headers["Authorization"] = "Basic %s" % base64string;
    if "GET" == method:
        headers["Content-Type"] = "application/x-www-form-urlencoded"
        conn.request(method=method, url=endpoint, headers=headers)
    else :
        headers["Content-Type"] = "application/json"
        conn.request(method=method, url=endpoint, body=params, headers=headers)
    response = conn.getresponse()
    res = response.read()
    return res
def httpGet(host, endpoint, username="", password=""):
    return httpRequest("GET", host, endpoint, "", username, password)
def httpPost(host, endpoint, params, username="", password=""):
    return httpRequest("POST", host, endpoint, params, username, password)
def httpPut(host, endpoint, params, username="", password=""):
    return httpRequest("PUT", host, endpoint, params, username, password)
def getIndices(host, username="", password=""):
    endpoint = "/_cat/indices"
    indicesResult = httpGet(oldClusterHost, endpoint, oldClusterUserName, oldClusterPassword)
    indicesList = indicesResult.split("\n")
    indexList = []
    for indices in indicesList:
        if (indices.find("open") > 0):
            indexList.append(indices.split()[2])
    return indexList
def getSettings(index, host, username="", password=""):
    endpoint = "/" + index + "/_settings"
    indexSettings = httpGet(host, endpoint, username, password)
    print index + "  原始settings如下：\n" + indexSettings
    settingsDict = json.loads(indexSettings)
    ## 分片数默认和旧集群索引保持一致
    number_of_shards = settingsDict[index]["settings"]["index"]["number_of_shards"]
    ## 副本数默认为0
    number_of_replicas = DEFAULT_REPLICAS
    newSetting = "\"settings\": {\"number_of_shards\": %s, \"number_of_replicas\": %s}" % (number_of_shards, number_of_replicas)
    return newSetting
def getMapping(index, host, username="", password=""):
    endpoint = "/" + index + "/_mapping"
    indexMapping = httpGet(host, endpoint, username, password)
    print index + " 原始mapping如下：\n" + indexMapping
    mappingDict = json.loads(indexMapping)
    mappings = json.dumps(mappingDict[index]["mappings"])
    newMapping = "\"mappings\" : " + mappings
    return newMapping
def createIndexStatement(oldIndexName):
    settingStr = getSettings(oldIndexName, oldClusterHost, oldClusterUserName, oldClusterPassword)
    mappingStr = getMapping(oldIndexName, oldClusterHost, oldClusterUserName, oldClusterPassword)
    createstatement = "{\n" + str(settingStr) + ",\n" + str(mappingStr) + "\n}"
    return createstatement
def createIndex(oldIndexName, newIndexName=""):
    if (newIndexName == "") :
        newIndexName = oldIndexName
    createstatement = createIndexStatement(oldIndexName)
    print "新索引 " + newIndexName + " 的setting和mapping如下：\n" + createstatement
    endpoint = "/" + newIndexName
    createResult = httpPut(newClusterHost, endpoint, createstatement, newClusterUser, newClusterPassword)
    print "新索引 " + newIndexName + " 创建结果：" + createResult
## main
indexList = getIndices(oldClusterHost, oldClusterUserName, oldClusterPassword)
systemIndex = []
for index in indexList:
    if (index.startswith(".")):
        systemIndex.append(index)
    else :
        createIndex(index, index)
if (len(systemIndex) > 0) :
    for index in systemIndex:
        print index + " 或许是系统索引，不会重新创建，如有需要，请单独处理～"
```

## 迁移数据

以下提供了三种数据迁移的方式，请根据迁移的数据量大小以及实际业务情况，选择合适的方式迁移数据。

**说明：**

-   为保证数据迁移前后的一致性，需要上游业务停止自建ES集群的写操作，读服务才可以正常进行。迁移完毕后，直接切换到阿里云ES集群进行读写操作。如果不停止写操作可能会导致迁移前后数据不一致的问题。
-   使用以下方案迁移数据时，如果是通过`IP:Port`的方式访问自建ES集群，则必须在阿里云ES集群的YML文件中配置`reindex`白名单，添加自建ES集群的IP地址，例如`reindex.remote.whitelist: 1.1.1.1:9200,1.2.*.*:*`，详情请参见[修改YML参数配置](/intl.zh-CN/实例管理/ES集群配置/配置YML文件/修改YML参数配置.md)和[配置reindex白名单](/intl.zh-CN/实例管理/ES集群配置/配置YML文件/自定义reindex远程重建索引配置.md)。
-   当使用域名访问自建ES集群或阿里云ES集群时，不允许通过`http://host:port/path`这种带`path`的形式访问。

-   数据量小

    使用`reindex.sh`脚本。

    ```
    #!/bin/bash
    # file:reindex.sh
    indexName="您的索引名"
    newClusterUser="新集群用户名"
    newClusterPass="新集群密码"
    newClusterHost="新集群host"
    oldClusterUser="旧集群用户名"
    oldClusterPass="旧集群密码"
    # 旧集群host必须是[scheme]://[host]:[port]，例如http://10.37.1.*:9200
    oldClusterHost="旧集群host"
    curl -u ${newClusterUser}:${newClusterPass} -XPOST "http://${newClusterHost}/_reindex?pretty" -H "Content-Type: application/json" -d'{
        "source": {
            "remote": {
                "host": "'${oldClusterHost}'",
                "username": "'${oldClusterUser}'",
                "password": "'${oldClusterPass}'"
            },
            "index": "'${indexName}'",
            "query": {
                "match_all": {}
            }
        },
        "dest": {
           "index": "'${indexName}'"
        }
    }'
    ```

-   数据量大、无删除操作、有更新时间

    数据量较大且无删除操作时，可以使用滚动迁移的方式，减少停止写服务的时间。滚动迁移需要有一个类似于更新时间的字段代表新数据的写时序。可以在数据迁移完成后再停止写服务，然后快速更新一次，即可切换到阿里云ES集群恢复读写。

    ```
    #!/bin/bash
    # file: circleReindex.sh
    # CONTROLLING STARTUP:
    # 这是通过reindex操作远程重建索引的脚本，要求：
    # 1. 新集群已经创建完索引，或者支持自动创建和动态映射
    # 2. 新集群必须在yml里配置IP白名单 reindex.remote.whitelist: 172.16.123.*:9200
    # 3. host必须是[scheme]://[host]:[port]
    USAGE="Usage: sh circleReindex.sh <count>
           count: 执行次数，多次（负数为循环）增量执行或者单次执行
    Example:
            sh circleReindex.sh 1
            sh circleReindex.sh 5
            sh circleReindex.sh -1"
    indexName="您的索引名"
    newClusterUser="新集群用户名"
    newClusterPass="新集群密码"
    oldClusterUser="旧集群用户名"
    oldClusterPass="旧集群密码"
    ## http://myescluster.com
    newClusterHost="新集群host"
    # 旧集群host必须是[scheme]://[host]:[port]，例如http://10.37.1.*:9200
    oldClusterHost="旧集群host"
    timeField="更新时间字段"
    reindexTimes=0
    lastTimestamp=0
    curTimestamp=`date +%s`
    hasError=false
    function reIndexOP() {
        reindexTimes=$[${reindexTimes} + 1]
        curTimestamp=`date +%s`
        ret=`curl -u ${newClusterUser}:${newClusterPass} -XPOST "${newClusterHost}/_reindex?pretty" -H "Content-Type: application/json" -d '{
            "source": {
                "remote": {
                    "host": "'${oldClusterHost}'",
                    "username": "'${oldClusterUser}'",
                    "password": "'${oldClusterPass}'"
                },
                "index": "'${indexName}'",
                "query": {
                    "range" : {
                        "'${timeField}'" : {
                            "gte" : '${lastTimestamp}',
                            "lt" : '${curTimestamp}'
                        }
                    }
                }
            },
            "dest": {
                "index": "'${indexName}'"
            }
        }'`
        lastTimestamp=${curTimestamp}
        echo "第${reindexTimes}次reIndex，本次更新截止时间 ${lastTimestamp} 结果：${ret}"
        if [[ ${ret} == *error* ]]; then
            hasError=true
            echo "本次执行异常，中断后续执行操作～～，请检查"
        fi
    }
    function start() {
        ## 负数就不停循环执行
        if [[ $1 -lt 0 ]]; then
            while :
            do
                reIndexOP
            done
        elif [[ $1 -gt 0 ]]; then
            k=0
            while [[ k -lt $1 ]] && [[ ${hasError} == false ]]; do
                reIndexOP
                let ++k
            done
        fi
    }
    ## main 
    if [ $# -lt 1 ]; then
        echo "$USAGE"
        exit 1
    fi
    echo "开始执行索引 ${indexName} 的 ReIndex操作"
    start $1
    echo "总共执行 ${reindexTimes} 次 reIndex 操作"
    ```

-   数据量大、无删除操作、无更新时间

    当数据量较大，且索引的mapping中没有定义更新时间的字段时，需要由上游业务修改代码添加更新时间的字段。添加完成后可以先将历史数据迁移完，然后再使用上述第二种方案进行操作。

    ```
    #!/bin/bash
    # file:miss.sh
    indexName="您的索引名"
    newClusterUser="新集群用户名"
    newClusterPass="新集群密码"
    newClusterHost="新集群host"
    oldClusterUser="旧集群用户名"
    oldClusterPass="旧集群密码"
    # 旧集群host必须是[scheme]://[host]:[port]，例如http://10.37.1.*:9200
    oldClusterHost="旧集群host"
    timeField="updatetime"
    curl -u ${newClusterUser}:${newClusterPass} -XPOST "http://${newClusterHost}/_reindex?pretty" -H "Content-Type: application/json" -d '{
        "source": {
            "remote": {
                "host": "'${oldClusterHost}'",
                "username": "'${oldClusterUser}'",
                "password": "'${oldClusterPass}'"
            },
            "index": "'${indexName}'",
            "query": {
                "bool": {
                    "must_not": {
                        "exists": {
                            "field": "'${timeField}'"
                        }
                    }
                }
            }
        },
        "dest": {
           "index": "'${indexName}'"
        }
    }'
    ```

-   不停止写服务。

    敬请期待。


## 常见问题

-   问题：执行curl命令时提示`{"error":"Content-Type header [application/x-www-form-urlencoded] is not supported","status":406}`。

    解决方法：可以在curl命令中添加`-H "Content-Type: application/json"`参数重试。

    ```
      // 获取旧集群中所有索引信息，如果没有权限可去掉"-u user:pass"参数，oldClusterHost为旧集群的host，注意替换。
      curl -u user:pass -XGET http://oldClusterHost/_cat/indices | awk '{print $3}'
      // 参考上面返回的索引列表，获取需要迁移的指定用户索引的setting和mapping，注意替换indexName为要查询的用户索引名。
      curl -u user:pass -XGET http://oldClusterHost/indexName/_settings,_mapping?pretty=true
      // 参考上面获取到的对应索引的_settings和_mapping信息，在新集群中创建对应索引，索引副本数可以先设置为0，用于加快数据同步速度，数据迁移完成后再重置副本数为1。
      //其中newClusterHost是新集群的host，testindex是已经创建的索引名，testtype是对应索引的type。
      curl -u user:pass -XPUT http://<newClusterHost>/<testindex> -d '{
        "testindex" : {
            "settings" : {
                "number_of_shards" : "5", //假设旧集群中对应索引的shard数是5个。
                "number_of_replicas" : "0" //设置索引副本为0。
              }
            },
            "mappings" : { //假设旧集群中对应索引的mappings配置如下。
                "testtype" : {
                    "properties" : {
                        "uid" : {
                            "type" : "long"
                        },
                        "name" : {
                            "type" : "text"
                        },
                        "create_time" : {
                          "type" : "long"
                        }
                    }
               }
           }
       }
    }'
    ```

-   问题：数据同步速度慢。

    解决方法： 如果单索引数据量比较大，可以在迁移前将目标索引的副本数设置为 0，刷新时间为-1。待数据迁移完成后，再更改回来，这样可以加快数据同步速度。

    ```
    // 迁移索引数据前可以先将索引副本数设为0，不刷新，用于加快数据迁移速度。
    curl -u user:password -XPUT 'http://<host:port>/indexName/_settings' -d' {
            "number_of_replicas" : 0,
            "refresh_interval" : "-1"
    }'
    // 索引数据迁移完成后，可以重置索引副本数为1，刷新时间1s（1s是默认值）。
    curl -u user:password -XPUT 'http://<host:port>/indexName/_settings' -d' {
            "number_of_replicas" : 1,
            "refresh_interval" : "1s"
    }'
    ```


