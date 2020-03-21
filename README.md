# FlameScope Playground

### FlameScope
https://github.com/Netflix/flamescope
https://netflixtechblog.com/netflix-flamescope-a57ca19d47bb
http://www.brendangregg.com/blog/2018-11-08/flamescope-pattern-recognition.html

### FlameGraphs
https://github.com/brendangregg/FlameGraph
https://github.com/jvm-profiling-tools/async-profiler
https://medium.com/@maheshsenni/java-performance-profiling-using-flame-graphs-e29238130375

### PERF
https://perf.wiki.kernel.org/index.php/Tutorial
http://www.brendangregg.com/perf.html
https://shuheikagawa.com/blog/2018/09/16/node-js-under-a-microscope/

### PERF-MAP-AGENT
https://github.com/jvm-profiling-tools/perf-map-agent

## Create FlameGraphs

-XX:+UnlockDiagnosticVMOptions -XX:+DebugNonSafepoints

Run FlameScope:
```
docker run --rm -it -v ~/flamescope:/profiles:ro -p 5000:5000 flamescope
```

```
# Run application
java -cp ./target/flamescope-1.0-SNAPSHOT.jar pbouda.flamescope.Scheduler
java -XX:+PreserveFramePointer -cp ./target/flamescope-1.0-SNAPSHOT.jar pbouda.flamescope.Scheduler

# Run Perf_events
perf record -F 99 -p `pgrep -f Scheduler` -g -- sleep 60

# Get Symbols from JAVA 
create-java-perf-map.sh `pgrep -f Scheduler`

# Make binary dump human readible
perf script > out.perf

# Folding stacktraces
stackcollapse-perf.pl --all out.perf > out.folded

# Create flamegraph
flamegraph.pl --color=java --hash out.folded > flamegraph.svg
```