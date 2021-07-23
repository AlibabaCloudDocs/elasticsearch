# Performance test of an Elasticsearch cluster with three 8-vCPU 32-GiB data nodes

This topic lists the performance metrics of an Elasticsearch cluster that contains three data nodes. Each data node has 8 vCPUs and 32 GiB of memory. The metrics include the Kibana metrics during the performance test and the performance metrics that are used to calculate these Kibana metrics.

**Note:** The official [GeoNames](http://www.geonames.org/) database is used for the test. The database contains 11,520,617 DOC files whose total size is 3.3 GB.

## Kibana metrics during the performance test

![Kibana metrics of an Elasticsearch cluster with three 8-vCPU 32-GiB data nodes](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9119740951/p54046.png)

## Performance metrics

|Metric|Operation|Value|Unit|
|------|---------|-----|----|
|Cumulative indexing time of primary shards|None|42.3229|min|
|Min cumulative indexing time across primary shards|None|0.000133333|min|
|Median cumulative indexing time across primary shards|None|6.85448|min|
|Max cumulative indexing time across primary shards|None|7.15663|min|
|Cumulative indexing throttle time of primary shards|None|0|min|
|Min cumulative indexing throttle time across primary shards|None|0|min|
|Median cumulative indexing throttle time across primary shards|None|0|min|
|Max cumulative indexing throttle time across primary shards|None|0|min|
|Cumulative merge time of primary shards|None|14.212|min|
|Cumulative merge count of primary shards|None|897|items|
|Min cumulative merge time across primary shards|None|0|min|
|Median cumulative merge time across primary shards|None|2.0934|min|
|Max cumulative merge time across primary shards|None|2.1898|min|
|Cumulative merge throttle time of primary shards|None|1.16502|min|
|Min cumulative merge throttle time across primary shards|None|0|min|
|Median cumulative merge throttle time across primary shards|None|0.157883|min|
|Max cumulative merge throttle time across primary shards|None|0.238017|min|
|Cumulative refresh time of primary shards|None|3.59033|min|
|Cumulative refresh count of primary shards|None|6963|items|
|Min cumulative refresh time across primary shards|None|0.000366667|min|
|Median cumulative refresh time across primary shards|None|0.401783|min|
|Max cumulative refresh time across primary shards|None|0.8976|min|
|Cumulative flush time of primary shards|None|0.514533|min|
|Cumulative flush count of primary shards|None|17|items|
|Min cumulative flush time across primary shards|None|0|min|
|Median cumulative flush time across primary shards|None|0.0558833|min|
|Max cumulative flush time across primary shards|None|0.102767|min|
|Total Young Gen GC|None|2.867|s|
|Total Old Gen GC|None|0|s|
|Store size|None|3.36733|GB|
|Translog size|None|3.40745|GB|
|Heap used for segments|None|20.2287|MB|
|Heap used for doc values|None|0.102116|MB|
|Heap used for terms|None|18.976|MB|
|Heap used for norms|None|0.0888672|MB|
|Heap used for points|None|0.286792|MB|
|Heap used for stored fields|None|0.774918|MB|
|Segment count|None|135|items|
|Min Throughput|index-append|92809.2|docs/s|
|Median Throughput|index-append|92809.2|docs/s|
|Max Throughput|index-append|92809.2|docs/s|
|50th percentile latency|index-append|1092.82|ms|
|90th percentile latency|index-append|1482.98|ms|
|100th percentile latency|index-append|1655.12|ms|
|50th percentile service time|index-append|1092.82|ms|
|90th percentile service time|index-append|1482.98|ms|
|100th percentile service time|index-append|1655.12|ms|
|error rate|index-append|0|%|
|Min Throughput|index-stats|90.03|ops/s|
|Median Throughput|index-stats|90.04|ops/s|
|Max Throughput|index-stats|90.07|ops/s|
|50th percentile latency|index-stats|6.66079|ms|
|90th percentile latency|index-stats|7.18313|ms|
|99th percentile latency|index-stats|8.61447|ms|
|99.9th percentile latency|index-stats|14.5834|ms|
|100th percentile latency|index-stats|14.7291|ms|
|50th percentile service time|index-stats|6.58463|ms|
|90th percentile service time|index-stats|7.08993|ms|
|99th percentile service time|index-stats|8.03104|ms|
|99.9th percentile service time|index-stats|14.5087|ms|
|100th percentile service time|index-stats|14.6548|ms|
|error rate|index-stats|0|%|
|Min Throughput|node-stats|90.03|ops/s|
|Median Throughput|node-stats|90.05|ops/s|
|Max Throughput|node-stats|90.18|ops/s|
|50th percentile latency|node-stats|6.75975|ms|
|90th percentile latency|node-stats|7.45763|ms|
|99th percentile latency|node-stats|9.49112|ms|
|99.9th percentile latency|node-stats|21.5716|ms|
|100th percentile latency|node-stats|23.0975|ms|
|50th percentile service time|node-stats|6.68247|ms|
|90th percentile service time|node-stats|7.34734|ms|
|99th percentile service time|node-stats|8.80335|ms|
|99.9th percentile service time|node-stats|19.4196|ms|
|100th percentile service time|node-stats|23.0214|ms|
|error rate|node-stats|0|%|
|Min Throughput|default|50|ops/s|
|Median Throughput|default|50.01|ops/s|
|Max Throughput|default|50.03|ops/s|
|50th percentile latency|default|13.4787|ms|
|90th percentile latency|default|16.4997|ms|
|99th percentile latency|default|21.6313|ms|
|99.9th percentile latency|default|27.411|ms|
|100th percentile latency|default|27.7961|ms|
|50th percentile service time|default|13.3791|ms|
|90th percentile service time|default|15.4398|ms|
|99th percentile service time|default|21.5527|ms|
|99.9th percentile service time|default|27.3316|ms|
|100th percentile service time|default|27.7195|ms|
|error rate|default|0|%|
|Min Throughput|term|200.02|ops/s|
|Median Throughput|term|200.03|ops/s|
|Max Throughput|term|200.03|ops/s|
|50th percentile latency|term|4.39089|ms|
|90th percentile latency|term|4.51443|ms|
|99th percentile latency|term|5.61225|ms|
|99.9th percentile latency|term|10.4551|ms|
|100th percentile latency|term|10.6584|ms|
|50th percentile service time|term|4.32217|ms|
|90th percentile service time|term|4.43736|ms|
|99th percentile service time|term|4.61988|ms|
|99.9th percentile service time|term|5.42257|ms|
|100th percentile service time|term|5.83687|ms|
|error rate|term|0|%|
|Min Throughput|phrase|176.79|ops/s|
|Median Throughput|phrase|179.33|ops/s|
|Max Throughput|phrase|180.82|ops/s|
|50th percentile latency|phrase|581.347|ms|
|90th percentile latency|phrase|754.845|ms|
|99th percentile latency|phrase|791.243|ms|
|99.9th percentile latency|phrase|794.407|ms|
|100th percentile latency|phrase|794.55|ms|
|50th percentile service time|phrase|5.38911|ms|
|90th percentile service time|phrase|5.61755|ms|
|99th percentile service time|phrase|6.07193|ms|
|99.9th percentile service time|phrase|10.8131|ms|
|100th percentile service time|phrase|11.2903|ms|
|error rate|phrase|0|%|
|Min Throughput|country\_agg\_uncached|4|ops/s|
|Median Throughput|country\_agg\_uncached|4.01|ops/s|
|Max Throughput|country\_agg\_uncached|4.01|ops/s|
|50th percentile latency|country\_agg\_uncached|145.729|ms|
|90th percentile latency|country\_agg\_uncached|157.922|ms|
|99th percentile latency|country\_agg\_uncached|169.1|ms|
|100th percentile latency|country\_agg\_uncached|171.331|ms|
|50th percentile service time|country\_agg\_uncached|145.573|ms|
|90th percentile service time|country\_agg\_uncached|157.744|ms|
|99th percentile service time|country\_agg\_uncached|168.924|ms|
|100th percentile service time|country\_agg\_uncached|171.149|ms|
|error rate|country\_agg\_uncached|0|%|
|Min Throughput|country\_agg\_cached|100.02|ops/s|
|Median Throughput|country\_agg\_cached|100.05|ops/s|
|Max Throughput|country\_agg\_cached|100.09|ops/s|
|50th percentile latency|country\_agg\_cached|4.72138|ms|
|90th percentile latency|country\_agg\_cached|4.97254|ms|
|99th percentile latency|country\_agg\_cached|5.36374|ms|
|99.9th percentile latency|country\_agg\_cached|18.7445|ms|
|100th percentile latency|country\_agg\_cached|21.2341|ms|
|50th percentile service time|country\_agg\_cached|4.64525|ms|
|90th percentile service time|country\_agg\_cached|4.89226|ms|
|99th percentile service time|country\_agg\_cached|5.18021|ms|
|99.9th percentile service time|country\_agg\_cached|14.9384|ms|
|100th percentile service time|country\_agg\_cached|21.1512|ms|
|error rate|country\_agg\_cached|0|%|
|Min Throughput|scroll|20.04|pages/s|
|Median Throughput|scroll|20.05|pages/s|
|Max Throughput|scroll|20.06|pages/s|
|50th percentile latency|scroll|514.808|ms|
|90th percentile latency|scroll|550.392|ms|
|99th percentile latency|scroll|576.239|ms|
|100th percentile latency|scroll|584.526|ms|
|50th percentile service time|scroll|514.028|ms|
|90th percentile service time|scroll|549.609|ms|
|99th percentile service time|scroll|575.47|ms|
|100th percentile service time|scroll|583.767|ms|
|error rate|scroll|0|%|
|Min Throughput|expression|2|ops/s|
|Median Throughput|expression|2|ops/s|
|Max Throughput|expression|2|ops/s|
|50th percentile latency|expression|460.795|ms|
|90th percentile latency|expression|482.177|ms|
|99th percentile latency|expression|491.312|ms|
|100th percentile latency|expression|493.126|ms|
|50th percentile service time|expression|460.65|ms|
|90th percentile service time|expression|481.979|ms|
|99th percentile service time|expression|491.119|ms|
|100th percentile service time|expression|492.936|ms|
|error rate|expression|0|%|
|Min Throughput|painless\_static|1.5|ops/s|
|Median Throughput|painless\_static|1.5|ops/s|
|Max Throughput|painless\_static|1.5|ops/s|
|50th percentile latency|painless\_static|493.747|ms|
|90th percentile latency|painless\_static|513.06|ms|
|99th percentile latency|painless\_static|601.632|ms|
|100th percentile latency|painless\_static|678.591|ms|
|50th percentile service time|painless\_static|493.504|ms|
|90th percentile service time|painless\_static|512.17|ms|
|99th percentile service time|painless\_static|601.373|ms|
|100th percentile service time|painless\_static|678.352|ms|
|error rate|painless\_static|0|%|
|Min Throughput|painless\_dynamic|1.5|ops/s|
|Median Throughput|painless\_dynamic|1.5|ops/s|
|Max Throughput|painless\_dynamic|1.5|ops/s|
|50th percentile latency|painless\_dynamic|492.847|ms|
|90th percentile latency|painless\_dynamic|507.818|ms|
|99th percentile latency|painless\_dynamic|528.414|ms|
|100th percentile latency|painless\_dynamic|529.858|ms|
|50th percentile service time|painless\_dynamic|492.553|ms|
|90th percentile service time|painless\_dynamic|507.568|ms|
|99th percentile service time|painless\_dynamic|528.154|ms|
|100th percentile service time|painless\_dynamic|529.592|ms|
|error rate|painless\_dynamic|0|%|
|Min Throughput|large\_terms|1.5|ops/s|
|Median Throughput|large\_terms|1.5|ops/s|
|Max Throughput|large\_terms|1.5|ops/s|
|50th percentile latency|large\_terms|567.113|ms|
|90th percentile latency|large\_terms|586.85|ms|
|99th percentile latency|large\_terms|704.151|ms|
|100th percentile latency|large\_terms|757.566|ms|
|50th percentile service time|large\_terms|566.891|ms|
|90th percentile service time|large\_terms|584.571|ms|
|99th percentile service time|large\_terms|678.04|ms|
|100th percentile service time|large\_terms|757.381|ms|
|error rate|large\_terms|0|%|
|Min Throughput|large\_filtered\_terms|1.5|ops/s|
|Median Throughput|large\_filtered\_terms|1.5|ops/s|
|Max Throughput|large\_filtered\_terms|1.5|ops/s|
|50th percentile latency|large\_filtered\_terms|570.444|ms|
|90th percentile latency|large\_filtered\_terms|588.125|ms|
|99th percentile latency|large\_filtered\_terms|734.918|ms|
|100th percentile latency|large\_filtered\_terms|775.135|ms|
|50th percentile service time|large\_filtered\_terms|569.793|ms|
|90th percentile service time|large\_filtered\_terms|585.437|ms|
|99th percentile service time|large\_filtered\_terms|682.943|ms|
|100th percentile service time|large\_filtered\_terms|774.924|ms|
|error rate|large\_filtered\_terms|0|%|
|Min Throughput|large\_prohibited\_terms|1.5|ops/s|
|Median Throughput|large\_prohibited\_terms|1.5|ops/s|
|Max Throughput|large\_prohibited\_terms|1.5|ops/s|
|50th percentile latency|large\_prohibited\_terms|567.806|ms|
|90th percentile latency|large\_prohibited\_terms|586.277|ms|
|99th percentile latency|large\_prohibited\_terms|623.088|ms|
|100th percentile latency|large\_prohibited\_terms|636.81|ms|
|50th percentile service time|large\_prohibited\_terms|567.621|ms|
|90th percentile service time|large\_prohibited\_terms|586.127|ms|
|99th percentile service time|large\_prohibited\_terms|622.914|ms|
|100th percentile service time|large\_prohibited\_terms|636.642|ms|
|error rate|large\_prohibited\_terms|0|%|

