# Performance test of an Elasticsearch cluster with three 2-vCPU 8-GiB data nodes

This topic lists the performance metrics of an Elasticsearch cluster that contains three data nodes. Each data node has 2 vCPUs and 8 GiB of memory. The metrics include the Kibana metrics during the performance test and the performance metrics that are used to calculate these Kibana metrics.

**Note:** The official [GeoNames](http://www.geonames.org/) database is used for benchmarking. The total data size is 3.3 GiB, including up to 11,520,617 DOC files.

## Kibana metrics during the performance test

![Kibana metrics of an Elasticsearch cluster with three 2-vCPU 8-GiB data nodes](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7419740951/p39910.png)

## Performance metrics

|Metric|Operation|Value|Unit|
|------|---------|-----|----|
|Indexing time|None|23.9479|min|
|Merge time|None|14.3001|min|
|Refresh time|None|5.26405|min|
|Flush time|None|0.0308333|min|
|Merge throttle time|None|1.27945|min|
|Total Young Gen GC|None|183.74|s|
|Total Old Gen GC|None|1.125|s|
|Heap used for segments|None|18.8167|MB|
|Heap used for doc values|None|0.452751|MB|
|Heap used for terms|None|17.2004|MB|
|Heap used for norms|None|0.0852051|MB|
|Heap used for points|None|0.241465|MB|
|Heap used for stored fields|None|0.836876|MB|
|Segment count|None|140|items|
|Min Throughput|index-append|28115.4|docs/s|
|Median Throughput|index-append|28645.5|docs/s|
|Max Throughput|index-append|30037.8|docs/s|
|50th percentile latency|index-append|1447.76|ms|
|90th percentile latency|index-append|1847.05|ms|
|99th percentile latency|index-append|2264.68|ms|
|99.9th percentile latency|index-append|2515.95|ms|
|100th percentile latency|index-append|2608.68|ms|
|50th percentile service time|index-append|1447.76|ms|
|90th percentile service time|index-append|1847.05|ms|
|99th percentile service time|index-append|2264.68|ms|
|99.9th percentile service time|index-append|2515.95|ms|
|100th percentile service time|index-append|2608.68|ms|
|error rate|index-append|0|%|
|Min Throughput|force-merge|2.1|ops/s|
|Median Throughput|force-merge|2.1|ops/s|
|Max Throughput|force-merge|2.1|ops/s|
|100th percentile latency|force-merge|475.984|ms|
|100th percentile service time|force-merge|475.984|ms|
|error rate|force-merge|0|%|
|Min Throughput|index-stats|97.75|ops/s|
|Median Throughput|index-stats|100.05|ops/s|
|Max Throughput|index-stats|100.07|ops/s|
|50th percentile latency|index-stats|5.09015|ms|
|90th percentile latency|index-stats|10.7365|ms|
|99th percentile latency|index-stats|234.761|ms|
|99.9th percentile latency|index-stats|277.393|ms|
|100th percentile latency|index-stats|281.866|ms|
|50th percentile service time|index-stats|5.01096|ms|
|90th percentile service time|index-stats|5.30021|ms|
|99th percentile service time|index-stats|12.0005|ms|
|99.9th percentile service time|index-stats|141.631|ms|
|100th percentile service time|index-stats|150.153|ms|
|error rate|index-stats|0|%|
|Min Throughput|node-stats|100.01|ops/s|
|Median Throughput|node-stats|100.08|ops/s|
|Max Throughput|node-stats|100.49|ops/s|
|50th percentile latency|node-stats|4.90659|ms|
|90th percentile latency|node-stats|5.29285|ms|
|99th percentile latency|node-stats|29.3245|ms|
|99.9th percentile latency|node-stats|43.3885|ms|
|100th percentile latency|node-stats|44.6019|ms|
|50th percentile service time|node-stats|4.83552|ms|
|90th percentile service time|node-stats|5.12694|ms|
|99th percentile service time|node-stats|9.08739|ms|
|99.9th percentile service time|node-stats|39.744|ms|
|100th percentile service time|node-stats|44.5383|ms|
|error rate|node-stats|0|%|
|Min Throughput|default|47.83|ops/s|
|Median Throughput|default|48.28|ops/s|
|Max Throughput|default|48.73|ops/s|
|50th percentile latency|default|617.465|ms|
|90th percentile latency|default|1033.98|ms|
|99th percentile latency|default|1083.23|ms|
|99.9th percentile latency|default|1095.4|ms|
|100th percentile latency|default|1097.14|ms|
|50th percentile service time|default|18.646|ms|
|90th percentile service time|default|24.9381|ms|
|99th percentile service time|default|35.7667|ms|
|99.9th percentile service time|default|57.3679|ms|
|100th percentile service time|default|151.505|ms|
|error rate|default|0|%|
|Min Throughput|term|199.43|ops/s|
|Median Throughput|term|200.07|ops/s|
|Max Throughput|term|200.13|ops/s|
|50th percentile latency|term|2.9728|ms|
|90th percentile latency|term|7.10648|ms|
|99th percentile latency|term|22.4487|ms|
|99.9th percentile latency|term|29.0737|ms|
|100th percentile latency|term|29.6253|ms|
|50th percentile service time|term|2.87833|ms|
|90th percentile service time|term|3.08983|ms|
|99th percentile service time|term|19.9777|ms|
|99.9th percentile service time|term|29.0082|ms|
|100th percentile service time|term|29.5597|ms|
|error rate|term|0|%|
|Min Throughput|phrase|199.71|ops/s|
|Median Throughput|phrase|200.04|ops/s|
|Max Throughput|phrase|200.07|ops/s|
|50th percentile latency|phrase|3.61484|ms|
|90th percentile latency|phrase|16.5523|ms|
|99th percentile latency|phrase|31.394|ms|
|99.9th percentile latency|phrase|33.902|ms|
|100th percentile latency|phrase|34.5784|ms|
|50th percentile service time|phrase|3.47402|ms|
|90th percentile service time|phrase|3.90958|ms|
|99th percentile service time|phrase|19.3773|ms|
|99.9th percentile service time|phrase|22.7947|ms|
|100th percentile service time|phrase|27.8164|ms|
|error rate|phrase|0|%|
|Min Throughput|country\_agg\_uncached|4.63|ops/s|
|Median Throughput|country\_agg\_uncached|4.65|ops/s|
|Max Throughput|country\_agg\_uncached|4.67|ops/s|
|50th percentile latency|country\_agg\_uncached|14864.3|ms|
|90th percentile latency|country\_agg\_uncached|21046|ms|
|99th percentile latency|country\_agg\_uncached|22902|ms|
|99.9th percentile latency|country\_agg\_uncached|22997.6|ms|
|100th percentile latency|country\_agg\_uncached|23018.7|ms|
|50th percentile service time|country\_agg\_uncached|204.174|ms|
|90th percentile service time|country\_agg\_uncached|242.492|ms|
|99th percentile service time|country\_agg\_uncached|345.382|ms|
|99.9th percentile service time|country\_agg\_uncached|378.302|ms|
|100th percentile service time|country\_agg\_uncached|422.53|ms|
|error rate|country\_agg\_uncached|0|%|
|Min Throughput|country\_agg\_cached|98.37|ops/s|
|Median Throughput|country\_agg\_cached|100.06|ops/s|
|Max Throughput|country\_agg\_cached|100.13|ops/s|
|50th percentile latency|country\_agg\_cached|3.2638|ms|
|90th percentile latency|country\_agg\_cached|4.69259|ms|
|99th percentile latency|country\_agg\_cached|189.143|ms|
|99.9th percentile latency|country\_agg\_cached|249.851|ms|
|100th percentile latency|country\_agg\_cached|256.028|ms|
|50th percentile service time|country\_agg\_cached|3.18679|ms|
|90th percentile service time|country\_agg\_cached|3.42086|ms|
|99th percentile service time|country\_agg\_cached|20.4171|ms|
|99.9th percentile service time|country\_agg\_cached|117.273|ms|
|100th percentile service time|country\_agg\_cached|255.951|ms|
|error rate|country\_agg\_cached|0|%|
|Min Throughput|scroll|59.16|ops/s|
|Median Throughput|scroll|60.44|ops/s|
|Max Throughput|scroll|61.02|ops/s|
|50th percentile latency|scroll|168347|ms|
|90th percentile latency|scroll|240658|ms|
|99th percentile latency|scroll|257048|ms|
|100th percentile latency|scroll|258853|ms|
|50th percentile service time|scroll|402.962|ms|
|90th percentile service time|scroll|431.267|ms|
|99th percentile service time|scroll|455.632|ms|
|100th percentile service time|scroll|601.214|ms|
|error rate|scroll|0|%|
|Min Throughput|expression|2|ops/s|
|Median Throughput|expression|2|ops/s|
|Max Throughput|expression|2|ops/s|
|50th percentile latency|expression|409.417|ms|
|90th percentile latency|expression|434.858|ms|
|99th percentile latency|expression|501.498|ms|
|100th percentile latency|expression|517.438|ms|
|50th percentile service time|expression|409.165|ms|
|90th percentile service time|expression|434.749|ms|
|99th percentile service time|expression|498.681|ms|
|100th percentile service time|expression|517.332|ms|
|error rate|expression|0|%|
|Min Throughput|painless\_static|1.96|ops/s|
|Median Throughput|painless\_static|1.97|ops/s|
|Max Throughput|painless\_static|1.97|ops/s|
|50th percentile latency|painless\_static|3163.94|ms|
|90th percentile latency|painless\_static|3679.27|ms|
|99th percentile latency|painless\_static|3994.52|ms|
|100th percentile latency|painless\_static|4006.5|ms|
|50th percentile service time|painless\_static|503.588|ms|
|90th percentile service time|painless\_static|528.807|ms|
|99th percentile service time|painless\_static|600.103|ms|
|100th percentile service time|painless\_static|623.666|ms|
|error rate|painless\_static|0|%|
|Min Throughput|painless\_dynamic|2|ops/s|
|Median Throughput|painless\_dynamic|2|ops/s|
|Max Throughput|painless\_dynamic|2|ops/s|
|50th percentile latency|painless\_dynamic|611.305|ms|
|90th percentile latency|painless\_dynamic|786.806|ms|
|99th percentile latency|painless\_dynamic|973.432|ms|
|100th percentile latency|painless\_dynamic|982.484|ms|
|50th percentile service time|painless\_dynamic|494.097|ms|
|90th percentile service time|painless\_dynamic|518.082|ms|
|99th percentile service time|painless\_dynamic|606.748|ms|
|100th percentile service time|painless\_dynamic|638.903|ms|
|error rate|painless\_dynamic|0|%|
|Min Throughput|large\_filtered\_terms|1.39|ops/s|
|Median Throughput|large\_filtered\_terms|1.4|ops/s|
|Max Throughput|large\_filtered\_terms|1.4|ops/s|
|50th percentile latency|large\_filtered\_terms|65601.1|ms|
|90th percentile latency|large\_filtered\_terms|82494.7|ms|
|99th percentile latency|large\_filtered\_terms|86452.2|ms|
|100th percentile latency|large\_filtered\_terms|86857.3|ms|
|50th percentile service time|large\_filtered\_terms|707.17|ms|
|90th percentile service time|large\_filtered\_terms|747.949|ms|
|99th percentile service time|large\_filtered\_terms|847.069|ms|
|100th percentile service time|large\_filtered\_terms|927.917|ms|
|error rate|large\_filtered\_terms|0|%|
|Min Throughput|large\_prohibited\_terms|1.46|ops/s|
|Median Throughput|large\_prohibited\_terms|1.46|ops/s|
|Max Throughput|large\_prohibited\_terms|1.46|ops/s|
|50th percentile latency|large\_prohibited\_terms|55916.3|ms|
|90th percentile latency|large\_prohibited\_terms|70529.7|ms|
|99th percentile latency|large\_prohibited\_terms|73769.1|ms|
|100th percentile latency|large\_prohibited\_terms|74143.9|ms|
|50th percentile service time|large\_prohibited\_terms|679.394|ms|
|90th percentile service time|large\_prohibited\_terms|717.476|ms|
|99th percentile service time|large\_prohibited\_terms|782.085|ms|
|100th percentile service time|large\_prohibited\_terms|822.723|ms|
|error rate|large\_prohibited\_terms|0|%|

