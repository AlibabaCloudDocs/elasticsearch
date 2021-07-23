# Performance test of an Elasticsearch cluster with three 4-vCPU 16-GiB data nodes

This topic lists the performance metrics of an Elasticsearch cluster that contains three data nodes. Each data node has 4 vCPUs and 16 GiB of memory. The metrics include the Kibana metrics during the performance test and the performance metrics that are used to calculate these Kibana metrics.

**Note:** The official [GeoNames](http://www.geonames.org/) database is used for the test. The database contains 11,520,617 DOC files whose total size is 3.3 GB.

## Kibana metrics during the performance test

![Kibana metrics of an Elasticsearch cluster with three 4-vCPU 16-GiB data nodes](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4719740951/p39909.png)

## Performance metrics

|Metric|Operation|Value|Unit|
|------|---------|-----|----|
|Indexing time|None|26.3543|min|
|Merge time|None|11.0297|min|
|Refresh time|None|3.05238|min|
|Flush time|None|0.04485|min|
|Merge throttle time|None|1.39282|min|
|Total Young Gen GC|None|92.902|s|
|Total Old Gen GC|None|0.4|s|
|Heap used for segments|None|18.7955|MB|
|Heap used for doc values|None|0.360752|MB|
|Heap used for terms|None|17.2739|MB|
|Heap used for norms|None|0.0877075|MB|
|Heap used for points|None|0.241213|MB|
|Heap used for stored fields|None|0.831932|MB|
|Segment count|None|133|items|
|Min Throughput|index-append|51751.7|docs/s|
|Median Throughput|index-append|52303|docs/s|
|Max Throughput|index-append|54076.3|docs/s|
|50th percentile latency|index-append|743.939|ms|
|90th percentile latency|index-append|1045.7|ms|
|99th percentile latency|index-append|1325.21|ms|
|100th percentile latency|index-append|1794.39|ms|
|50th percentile service time|index-append|743.939|ms|
|90th percentile service time|index-append|1045.7|ms|
|99th percentile service time|index-append|1325.21|ms|
|100th percentile service time|index-append|1794.39|ms|
|error rate|index-append|0|%|
|Min Throughput|force-merge|0.95|ops/s|
|Median Throughput|force-merge|0.95|ops/s|
|Max Throughput|force-merge|0.95|ops/s|
|100th percentile latency|force-merge|1052.54|ms|
|100th percentile service time|force-merge|1052.54|ms|
|error rate|force-merge|0|%|
|Min Throughput|index-stats|100.04|ops/s|
|Median Throughput|index-stats|100.05|ops/s|
|Max Throughput|index-stats|100.09|ops/s|
|50th percentile latency|index-stats|4.85232|ms|
|90th percentile latency|index-stats|5.14185|ms|
|99th percentile latency|index-stats|77.3127|ms|
|99.9th percentile latency|index-stats|123.888|ms|
|100th percentile latency|index-stats|128.01|ms|
|50th percentile service time|index-stats|4.78006|ms|
|90th percentile service time|index-stats|4.9831|ms|
|99th percentile service time|index-stats|9.66475|ms|
|99.9th percentile service time|index-stats|48.4445|ms|
|100th percentile service time|index-stats|127.945|ms|
|error rate|index-stats|0|%|
|Min Throughput|node-stats|100.05|ops/s|
|Median Throughput|node-stats|100.1|ops/s|
|Max Throughput|node-stats|100.55|ops/s|
|50th percentile latency|node-stats|4.55259|ms|
|90th percentile latency|node-stats|4.78784|ms|
|99th percentile latency|node-stats|18.8034|ms|
|99.9th percentile latency|node-stats|43.7684|ms|
|100th percentile latency|node-stats|48.1474|ms|
|50th percentile service time|node-stats|4.48138|ms|
|90th percentile service time|node-stats|4.69386|ms|
|99th percentile service time|node-stats|5.64618|ms|
|99.9th percentile service time|node-stats|27.8155|ms|
|100th percentile service time|node-stats|43.6905|ms|
|error rate|node-stats|0|%|
|Min Throughput|default|49.81|ops/s|
|Median Throughput|default|50|ops/s|
|Max Throughput|default|50|ops/s|
|50th percentile latency|default|19.7245|ms|
|90th percentile latency|default|94.1457|ms|
|99th percentile latency|default|133.091|ms|
|99.9th percentile latency|default|137.285|ms|
|100th percentile latency|default|138.043|ms|
|50th percentile service time|default|19.1469|ms|
|90th percentile service time|default|19.9554|ms|
|99th percentile service time|default|25.3462|ms|
|99.9th percentile service time|default|54.7931|ms|
|100th percentile service time|default|133.771|ms|
|error rate|default|0|%|
|Min Throughput|term|200.05|ops/s|
|Median Throughput|term|200.08|ops/s|
|Max Throughput|term|200.12|ops/s|
|50th percentile latency|term|3.07948|ms|
|90th percentile latency|term|3.37296|ms|
|99th percentile latency|term|22.3272|ms|
|99.9th percentile latency|term|26.9648|ms|
|100th percentile latency|term|28.1562|ms|
|50th percentile service time|term|3.00599|ms|
|90th percentile service time|term|3.15279|ms|
|99th percentile service time|term|4.22302|ms|
|99.9th percentile service time|term|26.9017|ms|
|100th percentile service time|term|28.0823|ms|
|error rate|term|0|%|
|Min Throughput|phrase|199.84|ops/s|
|Median Throughput|phrase|200.04|ops/s|
|Max Throughput|phrase|200.09|ops/s|
|50th percentile latency|phrase|3.76927|ms|
|90th percentile latency|phrase|13.6055|ms|
|99th percentile latency|phrase|28.0245|ms|
|99.9th percentile latency|phrase|34.7198|ms|
|100th percentile latency|phrase|35.551|ms|
|50th percentile service time|phrase|3.67227|ms|
|90th percentile service time|phrase|4.08037|ms|
|99th percentile service time|phrase|16.9256|ms|
|99.9th percentile service time|phrase|24.4886|ms|
|100th percentile service time|phrase|29.8604|ms|
|error rate|phrase|0|%|
|Min Throughput|country\_agg\_uncached|4.95|ops/s|
|Median Throughput|country\_agg\_uncached|4.99|ops/s|
|Max Throughput|country\_agg\_uncached|5|ops/s|
|50th percentile latency|country\_agg\_uncached|330.923|ms|
|90th percentile latency|country\_agg\_uncached|2780.17|ms|
|99th percentile latency|country\_agg\_uncached|2866|ms|
|99.9th percentile latency|country\_agg\_uncached|2880.39|ms|
|100th percentile latency|country\_agg\_uncached|2882.11|ms|
|50th percentile service time|country\_agg\_uncached|197.883|ms|
|90th percentile service time|country\_agg\_uncached|213.402|ms|
|99th percentile service time|country\_agg\_uncached|256.649|ms|
|99.9th percentile service time|country\_agg\_uncached|290.496|ms|
|100th percentile service time|country\_agg\_uncached|296.875|ms|
|error rate|country\_agg\_uncached|0|%|
|Min Throughput|country\_agg\_cached|99.92|ops/s|
|Median Throughput|country\_agg\_cached|100.06|ops/s|
|Max Throughput|country\_agg\_cached|100.11|ops/s|
|50th percentile latency|country\_agg\_cached|3.30479|ms|
|90th percentile latency|country\_agg\_cached|3.52514|ms|
|99th percentile latency|country\_agg\_cached|52.8258|ms|
|99.9th percentile latency|country\_agg\_cached|112.895|ms|
|100th percentile latency|country\_agg\_cached|119.435|ms|
|50th percentile service time|country\_agg\_cached|3.23149|ms|
|90th percentile service time|country\_agg\_cached|3.41319|ms|
|99th percentile service time|country\_agg\_cached|7.60955|ms|
|99.9th percentile service time|country\_agg\_cached|26.2229|ms|
|100th percentile service time|country\_agg\_cached|119.365|ms|
|error rate|country\_agg\_cached|0|%|
|Min Throughput|scroll|61.59|ops/s|
|Median Throughput|scroll|61.67|ops/s|
|Max Throughput|scroll|61.94|ops/s|
|50th percentile latency|scroll|164549|ms|
|90th percentile latency|scroll|237443|ms|
|99th percentile latency|scroll|253860|ms|
|100th percentile latency|scroll|255710|ms|
|50th percentile service time|scroll|399.964|ms|
|90th percentile service time|scroll|424.303|ms|
|99th percentile service time|scroll|523.877|ms|
|100th percentile service time|scroll|639.45|ms|
|error rate|scroll|0|%|
|Min Throughput|expression|2|ops/s|
|Median Throughput|expression|2|ops/s|
|Max Throughput|expression|2|ops/s|
|50th percentile latency|expression|409.927|ms|
|90th percentile latency|expression|434.544|ms|
|99th percentile latency|expression|532.412|ms|
|100th percentile latency|expression|537.618|ms|
|50th percentile service time|expression|409.812|ms|
|90th percentile service time|expression|428.156|ms|
|99th percentile service time|expression|532.33|ms|
|100th percentile service time|expression|537.495|ms|
|error rate|expression|0|%|
|Min Throughput|painless\_static|2|ops/s|
|Median Throughput|painless\_static|2|ops/s|
|Max Throughput|painless\_static|2|ops/s|
|50th percentile latency|painless\_static|497.626|ms|
|90th percentile latency|painless\_static|643.32|ms|
|99th percentile latency|painless\_static|700.559|ms|
|100th percentile latency|painless\_static|704.679|ms|
|50th percentile service time|painless\_static|490.705|ms|
|90th percentile service time|painless\_static|500.663|ms|
|99th percentile service time|painless\_static|642.124|ms|
|100th percentile service time|painless\_static|683.621|ms|
|error rate|painless\_static|0|%|
|Min Throughput|painless\_dynamic|2|ops/s|
|Median Throughput|painless\_dynamic|2|ops/s|
|Max Throughput|painless\_dynamic|2|ops/s|
|50th percentile latency|painless\_dynamic|473.087|ms|
|90th percentile latency|painless\_dynamic|554.729|ms|
|99th percentile latency|painless\_dynamic|668.363|ms|
|100th percentile latency|painless\_dynamic|706.557|ms|
|50th percentile service time|painless\_dynamic|469.145|ms|
|90th percentile service time|painless\_dynamic|501.774|ms|
|99th percentile service time|painless\_dynamic|606.61|ms|
|100th percentile service time|painless\_dynamic|624.751|ms|
|error rate|painless\_dynamic|0|%|
|Min Throughput|large\_filtered\_terms|1.64|ops/s|
|Median Throughput|large\_filtered\_terms|1.64|ops/s|
|Max Throughput|large\_filtered\_terms|1.65|ops/s|
|50th percentile latency|large\_filtered\_terms|33013.5|ms|
|90th percentile latency|large\_filtered\_terms|40869|ms|
|99th percentile latency|large\_filtered\_terms|42644|ms|
|100th percentile latency|large\_filtered\_terms|42936.2|ms|
|50th percentile service time|large\_filtered\_terms|598.001|ms|
|90th percentile service time|large\_filtered\_terms|626.81|ms|
|99th percentile service time|large\_filtered\_terms|771.815|ms|
|100th percentile service time|large\_filtered\_terms|796.884|ms|
|error rate|large\_filtered\_terms|0|%|
|Min Throughput|large\_prohibited\_terms|1.69|ops/s|
|Median Throughput|large\_prohibited\_terms|1.69|ops/s|
|Max Throughput|large\_prohibited\_terms|1.7|ops/s|
|50th percentile latency|large\_prohibited\_terms|27732.3|ms|
|90th percentile latency|large\_prohibited\_terms|34305.5|ms|
|99th percentile latency|large\_prohibited\_terms|35840.4|ms|
|100th percentile latency|large\_prohibited\_terms|35993.5|ms|
|50th percentile service time|large\_prohibited\_terms|586.382|ms|
|90th percentile service time|large\_prohibited\_terms|618.185|ms|
|99th percentile service time|large\_prohibited\_terms|661.378|ms|
|100th percentile service time|large\_prohibited\_terms|823.782|ms|
|error rate|large\_prohibited\_terms|0|%|

