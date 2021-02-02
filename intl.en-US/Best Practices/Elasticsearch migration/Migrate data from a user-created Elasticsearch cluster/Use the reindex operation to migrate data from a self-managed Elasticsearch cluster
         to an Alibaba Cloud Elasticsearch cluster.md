# Use the reindex operation to migrate data from a self-managed Elasticsearch cluster to an Alibaba Cloud Elasticsearch cluster

This topic describes how to use the reindex operation to migrate data from a self-managed Elasticsearch cluster to an Alibaba Cloud Elasticsearch cluster. Related operations include index creation and data migration. The self-managed Elasticsearch cluster is running on an Elastic Compute Service \(ECS\) instance.

## Prerequisites

The self-managed Elasticsearch cluster meets the following requirements:

-   The ECS instance that hosts the self-managed Elasticsearch cluster is deployed in a virtual private cloud \(VPC\). You cannot use an ECS instance that is connected to a VPC over a ClassicLink. The self-managed Elasticsearch cluster and Alibaba Cloud Elasticsearch cluster are deployed in the same VPC.
-   The IP addresses of nodes in the Alibaba Cloud Elasticsearch cluster are added to the security group of the ECS instance that hosts the self-managed Elasticsearch cluster. You can query the IP addresses of the nodes in the Kibana console of the Alibaba Cloud Elasticsearch cluster. In addition, port 9200 is enabled.
-   The self-managed Elasticsearch cluster is connected to the Alibaba Cloud Elasticsearch cluster. You can test the connectivity by running the `curl -XGET http://<host>:9200` command on the server where you run scripts.

    **Note:** You can run all the scripts provided in this topic on a server that is connected to both clusters over port 9200.


## Background information

You can use the reindex operation to migrate data only to single-zone Alibaba Cloud Elasticsearch clusters. If you want to migrate data to a multi-zone Alibaba Cloud Elasticsearch cluster, we recommend that you use one of the following methods:

-   If the self-managed Elasticsearch cluster stores large volumes of data, use snapshots stored in Object Storage Service \(OSS\).
-   If you want to filter source data, use Logstash.

## Create indexes

Create indexes on the Alibaba Cloud Elasticsearch cluster based on the index settings of the self-managed Elasticsearch cluster. You can also enable the Auto Indexing feature for the Alibaba Cloud Elasticsearch cluster. However, we recommend that you do not use this feature.

The following sample code is a Python script used to create indexes on the Alibaba Cloud Elasticsearch cluster. By default, no replica shards are configured for these indexes.

```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
# File name: indiceCreate.py
import sys
import base64
import time
import httplib
import json
## Specify the host of the source Elasticsearch cluster.
oldClusterHost = "old-cluster.com"
## Specify the username of the source Elasticsearch cluster. The field can be empty.
oldClusterUserName = "old-username"
## Specify the password of the source Elasticsearch cluster. The field can be empty.
oldClusterPassword = "old-password"
## Specify the host of the destination Elasticsearch cluster.
newClusterHost = "new-cluster.com"
## Specify the username of the destination Elasticsearch cluster. The field can be empty.
newClusterUser = "new-username"
## Specify the password of the destination Elasticsearch cluster. The field can be empty.
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
    print index + "  Original settings: \n" + indexSettings
    settingsDict = json.loads(indexSettings)
    ## By default, the number of primary shards is the same as that for the indexes on the source Elasticsearch cluster.
    number_of_shards = settingsDict[index]["settings"]["index"]["number_of_shards"]
    ## The default number of replica shards is 0.
    number_of_replicas = DEFAULT_REPLICAS
    newSetting = "\"settings\": {\"number_of_shards\": %s, \"number_of_replicas\": %s}" % (number_of_shards, number_of_replicas)
    return newSetting
def getMapping(index, host, username="", password=""):
    endpoint = "/" + index + "/_mapping"
    indexMapping = httpGet(host, endpoint, username, password)
    print index + " Original mappings: \n" + indexMapping
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
    print "New index " + newIndexName + " Index settings and mappings: \n" + createstatement
    endpoint = "/" + newIndexName
    createResult = httpPut(newClusterHost, endpoint, createstatement, newClusterUser, newClusterPassword)
    print "New index " + newIndexName + " Creation result: " + createResult
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
        print index + " It may be a system index and will not be recreated. You can manually recreate the index as required."
```

## Migrate data

You can use one of the following methods to migrate data. Select a suitable method based on the volume of data that you want to migrate and your business requirements.

**Note:**

-   To ensure data consistency, you must stop writing data to the source Elasticsearch cluster before the migration. This way, you can continue to read data from the cluster. After the migration, you can read data from and write data to the destination Elasticsearch cluster.
-   If you connect to the source Elasticsearch cluster by using `IP address:Port number`, add the IP address of the ECS instance that hosts the source Elasticsearch cluster to the `reindex` whitelist of the destination Elasticsearch cluster. For example, add the following information to the YML file of the destination Elasticsearch cluster: `reindex.remote.whitelist: 1.1.1.1:9200,1.2.*.*:*`. For more information, see [Configure the YML file](/intl.en-US/Elasticsearch Instances Management/Elasticsearch cluster configuration/Configure the YML file.md).
-   If you connect to the source or destination Elasticsearch cluster by using its domain name, do not use the `http://host:port/path` format. `path` cannot be included.

-   Migrate a small volume of data

    Run the `reindex.sh` script.

    ```
    #!/bin/bash
    # file:reindex.sh
    indexName="The name of the index"
    newClusterUser="The username of the destination Elasticsearch cluster"
    newClusterPass="The password of the destination Elasticsearch cluster"
    newClusterHost="The host of the destination Elasticsearch cluster"
    oldClusterUser="The username of the source Elasticsearch cluster"
    oldClusterPass="The password of the source Elasticsearch cluster"
    # You must specify the host of the source Elasticsearch cluster in the format of [scheme]://[host]:[port]. Example: http://10.37.1.*:9200.
    oldClusterHost="The host of the source Elasticsearch cluster"
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

-   Migrate a large volume of data \(without deletions and with update time\)

    To migrate a large volume of data without deletions, you can perform a rolling update to shorten the time during which write operations are suspended. The rolling update requires that your data schema has a time-series attribute that indicates the update time. You can stop writing data to the source Elasticsearch cluster after data is migrated. Then, perform a rolling update to synchronize the data that is updated during the migration. After the rolling update is complete, you can read data from and write data to the destination Elasticsearch cluster.

    ```
    #!/bin/bash
    # file: circleReindex.sh
    # CONTROLLING STARTUP:
    # This is a script that uses the reindex operation to remotely reindex data. Requirements:
    # 1. Indexes are created on the destination Elasticsearch cluster, or the Auto Indexing and dynamic mapping features are enabled for the cluster.
    # 2. The following information is added to the YML file of the destination Elasticsearch cluster: reindex.remote.whitelist: 172.16.123.*:9200.
    # 3. The host is specified in the format of [scheme]://[host]:[port].
    USAGE="Usage: sh circleReindex.sh <count>
           count: the number of reindex operations that you can perform. A negative number indicates loop execution.
    Example:
            sh circleReindex.sh 1
            sh circleReindex.sh 5
            sh circleReindex.sh -1"
    indexName="The name of the index"
    newClusterUser="The username of the destination Elasticsearch cluster"
    newClusterPass="The password of the destination Elasticsearch cluster"
    oldClusterUser="The username of the source Elasticsearch cluster"
    oldClusterPass="The password of the source Elasticsearch cluster"
    ## http://myescluster.com
    newClusterHost="The host of the destination Elasticsearch cluster"
    # You must specify the host of the source Elasticsearch cluster in the format of [scheme]://[host]:[port]. Example: http://10.37.1.*:9200.
    oldClusterHost="The host of the source Elasticsearch cluster"
    timeField="The update time of data"
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
        echo "${reindexTimes} reindex operations are performed. The last reindex operation is complete at ${lastTimestamp}. Result: ${ret}."
        if [[ ${ret} == *error* ]]; then
            hasError=true
            echo "An unknown error occurred when you perform this operation. All subsequent operations are suspended."
        fi
    }
    function start() {
        ## A negative number indicates loop execution.
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
    echo "Start the reindex operation for the ${indexName} index."
    start $1
    echo "${reindexTimes} reindex operations are performed."
    ```

-   Migrate a large volume of data \(without deletions and update time\)

    You can migrate a large volume of data when no update time is defined in the index mappings of the source cluster. However, you must add an update time field to the index mappings. After the field is added, you can migrate existing data. Then, perform a rolling update that is described in the second data migration method to migrate incremental data.

    ```
    #!/bin/bash
    # file:miss.sh
    indexName="The name of the index"
    newClusterUser="The username of the destination Elasticsearch cluster"
    newClusterPass="The password of the destination Elasticsearch cluster"
    newClusterHost="The host of the destination Elasticsearch cluster"
    oldClusterUser="The username of the source Elasticsearch cluster"
    oldClusterPass="The password of the source Elasticsearch cluster"
    # You must specify the host of the source Elasticsearch cluster in the format of [scheme]://[host]:[port]. Example: http://10.37.1.*:9200.
    oldClusterHost="The host of the source Elasticsearch cluster"
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

-   Migrate data without the need to suspend write operations

    This data migration method will be available soon.


## FAQ

-   Problem: When I run the curl command, the system displays `{"error":"Content-Type header [application/x-www-form-urlencoded] is not supported","status":406}`.

    Solution: Add `-H "Content-Type: application/json"` to the curl command and try again.

    ```
      // Obtain all the indexes on the source cluster. If you do not have the required permissions, remove the "-u user:pass" parameter. Make sure that you have replaced oldClusterHost with the information about the host of the source Elasticsearch cluster.
      curl -u user:pass -XGET http://oldClusterHost/_cat/indices | awk '{print $3}'
      // Based on the returned indexes, obtain the settings and mappings of the index that you want to migrate for the specified user. Make sure that you have replaced indexName with the index name that you want to query.
      curl -u user:pass -XGET http://oldClusterHost/indexName/_settings,_mapping?pretty=true
      // Create an index on the destination Elasticsearch cluster based on the _settings and _mapping configurations that you obtained. You can set the number of replica shards to 0 to accelerate data migration, and change the number to 1 after data is migrated.
      // newClusterHost indicates the host of the destination Elasticsearch cluster, testindex indicates the name of the index that you have created, and testtype indicates the type of the index.
      curl -u user:pass -XPUT http://<newClusterHost>/<testindex> -d '{
        "testindex" : {
            "settings" : {
                "number_of_shards" : "5", //Specify the number of primary shards for the index on the source Elasticsearch cluster, such as 5.
                "number_of_replicas" : "0" //Set the number of replica shards to 0.
              }
            },
            "mappings" : { //Specify the mappings for the index on the source Elasticsearch cluster. Example:
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

-   Problem: Data migration is slow.

    Solution: If the source index stores large volumes of data, you can set the number of replica shards to 0 and the refresh interval to -1 for the destination index before migration. After migration, restore the settings to the original values. This accelerates data migration.

    ```
    // You can set the number of replica shards to 0 and disable the refresh feature to accelerate the migration.
    curl -u user:password -XPUT 'http://<host:port>/indexName/_settings' -d' {
            "number_of_replicas" : 0,
            "refresh_interval" : "-1"
    }'
    // After data is migrated, set the number of replica shards to 1 and the refresh interval to 1s (default value).
    curl -u user:password -XPUT 'http://<host:port>/indexName/_settings' -d' {
            "number_of_replicas" : 1,
            "refresh_interval" : "1s"
    }'
    ```


