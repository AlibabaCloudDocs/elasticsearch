# Comparison of stress testing results between an Elasticsearch cluster with three 4-vCPU 16-GiB data nodes and an Elasticsearch cluster with three 2-vCPU 8-GiB data nodes

This topic compares the stress testing results of two Elasticsearch clusters: a cluster with three 4-vCPU 16-GiB data nodes and a cluster with three 2-vCPU 8-GiB data nodes.

**Note:** The official [GeoNames](http://www.geonames.org/) database is used for the test. The database contains 11,520,617 DOC files whose total size is 3.3 GB.

|Metric|Operation|Cluster with 4-vCPU 16-GiB data nodes|Cluster with 2-vCPU 8-GiB data nodes|Difference|Unit|
|------|---------|-------------------------------------|------------------------------------|----------|----|
|Merge time|None|11.0297|14.3001|3.27042|min|
|Refresh time|None|3.05238|5.26405|2.21167|min|
|Flush time|None|0.04485|0.0308333|-0.01402|min|
|Merge throttle time|None|1.39282|1.27945|-0.11337|min|
|Total Young Gen GC|None|92.902|183.74|90.838|s|
|Total Old Gen GC|None|0.4|1.125|0.725|s|
|Heap used for segments|None|18.7955|18.8167|0.02126|MB|
|Heap used for doc values|None|0.360752|0.452751|0.092|MB|
|Heap used for terms|None|17.2739|17.2004|-0.07343|MB|
|Heap used for norms|None|0.0877075|0.0852051|-0.0025|MB|
|Heap used for points|None|0.241213|0.241465|0.00025|MB|
|Heap used for stored fields|None|0.831932|0.836876|0.00494|MB|
|Segment count|None|133|140|7|items|
|Min Throughput|index-append|51751.7|28115.4|-23636.2|docs/s|
|Median Throughput|index-append|52303|28645.6|-23657.5|docs/s|
|Max Throughput|index-append|54076.3|30037.8|-24038.5|docs/s|
|50th percentile latency|index-append|743.939|1447.76|703.818|ms|
|90th percentile latency|index-append|1045.7|1847.05|801.342|ms|
|99th percentile latency|index-append|1325.21|2264.68|939.47|ms|
|100th percentile latency|index-append|1794.39|2608.68|814.293|ms|
|50th percentile service time|index-append|743.939|1447.76|703.818|ms|
|90th percentile service time|index-append|1045.7|1847.05|801.342|ms|
|99th percentile service time|index-append|1325.21|2264.68|939.47|ms|
|100th percentile service time|index-append|1794.39|2608.68|814.293|ms|
|error rate|index-append|0|0|0|%|
|Min Throughput|force-merge|0.950072|2.10087|1.1508|ops/s|
|Median Throughput|force-merge|0.950072|2.10087|1.1508|ops/s|
|Max Throughput|force-merge|0.950072|2.10087|1.1508|ops/s|
|100th percentile latency|force-merge|1052.54|475.984|-576.556|ms|
|100th percentile service time|force-merge|1052.54|475.984|-576.556|ms|
|error rate|force-merge|0|0|0|%|
|Min Throughput|index-stats|100.037|97.7524|-2.28456|ops/s|
|Median Throughput|index-stats|100.049|100.048|-0.00112|ops/s|
|Max Throughput|index-stats|100.085|100.068|-0.01745|ops/s|
|50th percentile latency|index-stats|4.85232|5.09015|0.23784|ms|
|90th percentile latency|index-stats|5.14185|10.7365|5.59466|ms|
|99th percentile latency|index-stats|77.3127|234.761|157.448|ms|
|99.9th percentile latency|index-stats|123.888|277.393|153.505|ms|
|100th percentile latency|index-stats|128.01|281.866|153.856|ms|
|50th percentile service time|index-stats|4.78006|5.01096|0.23091|ms|
|90th percentile service time|index-stats|4.9831|5.30021|0.31711|ms|
|99th percentile service time|index-stats|9.66475|12.0005|2.33576|ms|
|99.9th percentile service time|index-stats|48.4445|141.631|93.186|ms|
|100th percentile service time|index-stats|127.945|150.153|22.2078|ms|
|error rate|index-stats|0|0|0|%|
|Min Throughput|node-stats|100.054|100.007|-0.04689|ops/s|
|Median Throughput|node-stats|100.098|100.085|-0.01341|ops/s|
|Max Throughput|node-stats|100.551|100.494|-0.0566|ops/s|
|50th percentile latency|node-stats|4.55259|4.90659|0.354|ms|
|90th percentile latency|node-stats|4.78784|5.29285|0.50501|ms|
|99th percentile latency|node-stats|18.8034|29.3245|10.5211|ms|
|99.9th percentile latency|node-stats|43.7684|43.3885|-0.3799|ms|
|100th percentile latency|node-stats|48.1474|44.6019|-3.54548|ms|
|50th percentile service time|node-stats|4.48138|4.83552|0.35414|ms|
|90th percentile service time|node-stats|4.69386|5.12694|0.43308|ms|
|99th percentile service time|node-stats|5.64618|9.08739|3.44121|ms|
|99.9th percentile service time|node-stats|27.8155|39.744|11.9285|ms|
|100th percentile service time|node-stats|43.6905|44.5383|0.84783|ms|
|error rate|node-stats|0|0|0|%|
|Min Throughput|default|49.8129|47.8334|-1.97948|ops/s|
|Median Throughput|default|50.0009|48.281|-1.71982|ops/s|
|Max Throughput|default|50.0045|48.7269|-1.2776|ops/s|
|50th percentile latency|default|19.7245|617.465|597.74|ms|
|90th percentile latency|default|94.1457|1033.98|939.834|ms|
|99th percentile latency|default|133.091|1083.23|950.137|ms|
|99.9th percentile latency|default|137.285|1095.4|958.114|ms|
|100th percentile latency|default|138.043|1097.14|959.1|ms|
|50th percentile service time|default|19.1469|18.646|-0.50082|ms|
|90th percentile service time|default|19.9554|24.9381|4.98271|ms|
|99th percentile service time|default|25.3462|35.7667|10.4206|ms|
|99.9th percentile service time|default|54.7931|57.3679|2.57481|ms|
|100th percentile service time|default|133.771|151.505|17.7337|ms|
|error rate|default|0|0|0|%|
|Min Throughput|term|200.055|199.431|-0.62401|ops/s|
|Median Throughput|term|200.076|200.072|-0.00349|ops/s|
|Max Throughput|term|200.119|200.13|0.01076|ops/s|
|50th percentile latency|term|3.07948|2.9728|-0.10668|ms|
|90th percentile latency|term|3.37296|7.10648|3.73353|ms|
|99th percentile latency|term|22.3272|22.4487|0.12153|ms|
|99.9th percentile latency|term|26.9648|29.0737|2.10889|ms|
|100th percentile latency|term|28.1562|29.6253|1.46915|ms|
|50th percentile service time|term|3.00599|2.87833|-0.12766|ms|
|90th percentile service time|term|3.15279|3.08983|-0.06296|ms|
|99th percentile service time|term|4.22302|19.9777|15.7546|ms|
|99.9th percentile service time|term|26.9017|29.0082|2.10648|ms|
|100th percentile service time|term|28.0823|29.5597|1.4774|ms|
|error rate|term|0|0|0|%|
|Min Throughput|phrase|199.842|199.711|-0.13145|ops/s|
|Median Throughput|phrase|200.04|200.038|-0.00174|ops/s|
|Max Throughput|phrase|200.087|200.074|-0.0125|ops/s|
|50th percentile latency|phrase|3.76927|3.61484|-0.15442|ms|
|90th percentile latency|phrase|13.6055|16.5523|2.94681|ms|
|99th percentile latency|phrase|28.0245|31.394|3.36944|ms|
|99.9th percentile latency|phrase|34.7198|33.902|-0.81778|ms|
|100th percentile latency|phrase|35.551|34.5784|-0.97253|ms|
|50th percentile service time|phrase|3.67227|3.47402|-0.19825|ms|
|90th percentile service time|phrase|4.08037|3.90958|-0.17079|ms|
|99th percentile service time|phrase|16.9256|19.3773|2.45168|ms|
|99.9th percentile service time|phrase|24.4886|22.7947|-1.69386|ms|
|100th percentile service time|phrase|29.8604|27.8164|-2.04399|ms|
|error rate|phrase|0|0|0|%|
|Min Throughput|country\_agg\_uncached|4.95005|4.6328|-0.31724|ops/s|
|Median Throughput|country\_agg\_uncached|4.99422|4.65258|-0.34163|ops/s|
|Max Throughput|country\_agg\_uncached|5.00022|4.67361|-0.32661|ops/s|
|50th percentile latency|country\_agg\_uncached|330.923|14864.3|14533.3|ms|
|90th percentile latency|country\_agg\_uncached|2780.17|21046|18265.8|ms|
|99th percentile latency|country\_agg\_uncached|2866|22902|20036|ms|
|99.9th percentile latency|country\_agg\_uncached|2880.39|22997.6|20117.2|ms|
|100th percentile latency|country\_agg\_uncached|2882.11|23018.7|20136.6|ms|
|50th percentile service time|country\_agg\_uncached|197.883|204.174|6.29064|ms|
|90th percentile service time|country\_agg\_uncached|213.402|242.492|29.0907|ms|
|99th percentile service time|country\_agg\_uncached|256.649|345.382|88.7335|ms|
|99.9th percentile service time|country\_agg\_uncached|290.496|378.302|87.8056|ms|
|100th percentile service time|country\_agg\_uncached|296.875|422.53|125.655|ms|
|error rate|country\_agg\_uncached|0|0|0|%|
|Min Throughput|country\_agg\_cached|99.9249|98.3659|-1.55896|ops/s|
|Median Throughput|country\_agg\_cached|100.064|100.056|-0.00795|ops/s|
|Max Throughput|country\_agg\_cached|100.112|100.135|0.02245|ops/s|
|50th percentile latency|country\_agg\_cached|3.30479|3.2638|-0.04099|ms|
|90th percentile latency|country\_agg\_cached|3.52514|4.69259|1.16745|ms|
|99th percentile latency|country\_agg\_cached|52.8258|189.143|136.317|ms|
|99.9th percentile latency|country\_agg\_cached|112.895|249.851|136.956|ms|
|100th percentile latency|country\_agg\_cached|119.435|256.028|136.593|ms|
|50th percentile service time|country\_agg\_cached|3.23149|3.18679|-0.0447|ms|
|90th percentile service time|country\_agg\_cached|3.41319|3.42086|0.00767|ms|
|99th percentile service time|country\_agg\_cached|7.60955|20.4171|12.8075|ms|
|99.9th percentile service time|country\_agg\_cached|26.2229|117.273|91.0502|ms|
|100th percentile service time|country\_agg\_cached|119.365|255.951|136.586|ms|
|error rate|country\_agg\_cached|0|0|0|%|
|Min Throughput|scroll|61.5897|59.1628|-2.42689|ops/s|
|Median Throughput|scroll|61.6735|60.4406|-1.23292|ops/s|
|Max Throughput|scroll|61.9387|61.019|-0.91967|ops/s|
|50th percentile latency|scroll|164549|168347|3798.13|ms|
|90th percentile latency|scroll|237443|240658|3214.79|ms|
|99th percentile latency|scroll|253860|257048|3187.91|ms|
|100th percentile latency|scroll|255710|258853|3143.03|ms|
|50th percentile service time|scroll|399.964|402.962|2.99858|ms|
|90th percentile service time|scroll|424.303|431.267|6.96397|ms|
|99th percentile service time|scroll|523.877|455.632|-68.2449|ms|
|100th percentile service time|scroll|639.45|601.214|-38.236|ms|
|error rate|scroll|0|0|0|%|
|Min Throughput|expression|1.9994|1.9998|0.0004|ops/s|
|Median Throughput|expression|2.00113|2.00113|0|ops/s|
|Max Throughput|expression|2.00186|2.00189|2e-05|ops/s|
|50th percentile latency|expression|409.927|409.417|-0.5091|ms|
|90th percentile latency|expression|434.544|434.858|0.31406|ms|
|99th percentile latency|expression|532.412|501.498|-30.914|ms|
|100th percentile latency|expression|537.618|517.438|-20.1798|ms|
|50th percentile service time|expression|409.812|409.165|-0.64674|ms|
|90th percentile service time|expression|428.156|434.749|6.59297|ms|
|99th percentile service time|expression|532.33|498.681|-33.6493|ms|
|100th percentile service time|expression|537.495|517.332|-20.1637|ms|
|error rate|expression|0|0|0|%|
|Min Throughput|painless\_static|1.99752|1.96306|-0.03446|ops/s|
|Median Throughput|painless\_static|1.99998|1.96607|-0.03391|ops/s|
|Max Throughput|painless\_static|2.00041|1.96914|-0.03127|ops/s|
|50th percentile latency|painless\_static|497.626|3163.94|2666.31|ms|
|90th percentile latency|painless\_static|643.32|3679.27|3035.95|ms|
|99th percentile latency|painless\_static|700.559|3994.52|3293.97|ms|
|100th percentile latency|painless\_static|704.679|4006.5|3301.82|ms|
|50th percentile service time|painless\_static|490.705|503.588|12.8834|ms|
|90th percentile service time|painless\_static|500.663|528.807|28.1439|ms|
|99th percentile service time|painless\_static|642.124|600.103|-42.021|ms|
|100th percentile service time|painless\_static|683.621|623.666|-59.9546|ms|
|error rate|painless\_static|0|0|0|%|
|Min Throughput|painless\_dynamic|1.99721|1.99513|-0.00209|ops/s|
|Median Throughput|painless\_dynamic|2.00032|1.99838|-0.00194|ops/s|
|Max Throughput|painless\_dynamic|2.00089|2.00053|-0.00036|ops/s|
|50th percentile latency|painless\_dynamic|473.087|611.305|138.218|ms|
|90th percentile latency|painless\_dynamic|554.729|786.806|232.077|ms|
|99th percentile latency|painless\_dynamic|668.363|973.432|305.069|ms|
|100th percentile latency|painless\_dynamic|706.557|982.484|275.926|ms|
|50th percentile service time|painless\_dynamic|469.145|494.097|24.9528|ms|
|90th percentile service time|painless\_dynamic|501.774|518.082|16.3086|ms|
|99th percentile service time|painless\_dynamic|606.61|606.748|0.13817|ms|
|100th percentile service time|painless\_dynamic|624.751|638.903|14.1524|ms|
|error rate|painless\_dynamic|0|0|0|%|
|Min Throughput|large\_filtered\_terms|1.64076|1.38866|-0.2521|ops/s|
|Median Throughput|large\_filtered\_terms|1.6443|1.39554|-0.24876|ops/s|
|Max Throughput|large\_filtered\_terms|1.65048|1.39764|-0.25283|ops/s|
|50th percentile latency|large\_filtered\_terms|33013.5|65601.1|32587.5|ms|
|90th percentile latency|large\_filtered\_terms|40869|82494.7|41625.7|ms|
|99th percentile latency|large\_filtered\_terms|42644|86452.2|43808.2|ms|
|100th percentile latency|large\_filtered\_terms|42936.2|86857.3|43921.1|ms|
|50th percentile service time|large\_filtered\_terms|598.001|707.17|109.169|ms|
|90th percentile service time|large\_filtered\_terms|626.81|747.949|121.139|ms|
|99th percentile service time|large\_filtered\_terms|771.815|847.069|75.2534|ms|
|100th percentile service time|large\_filtered\_terms|796.884|927.917|131.032|ms|
|error rate|large\_filtered\_terms|0|0|0|%|
|Min Throughput|large\_prohibited\_terms|1.6893|1.45607|-0.23323|ops/s|
|Median Throughput|large\_prohibited\_terms|1.69452|1.46074|-0.23379|ops/s|
|Max Throughput|large\_prohibited\_terms|1.69856|1.46248|-0.23608|ops/s|
|50th percentile latency|large\_prohibited\_terms|27732.3|55916.3|28184|ms|
|90th percentile latency|large\_prohibited\_terms|34305.5|70529.7|36224.2|ms|
|99th percentile latency|large\_prohibited\_terms|35840.4|73769.1|37928.7|ms|
|100th percentile latency|large\_prohibited\_terms|35993.5|74143.9|38150.4|ms|
|50th percentile service time|large\_prohibited\_terms|586.382|679.394|93.0121|ms|
|90th percentile service time|large\_prohibited\_terms|618.185|717.476|99.2908|ms|
|99th percentile service time|large\_prohibited\_terms|661.378|782.085|120.707|ms|
|100th percentile service time|large\_prohibited\_terms|823.782|822.723|-1.05804|ms|
|error rate|large\_prohibited\_terms|0|0|0|%|

