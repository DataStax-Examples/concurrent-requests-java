# Concurrent Requests in Java
This example shows three different ways of handling concurrent request execution using the DataStax Java driver.  


Contributors: [Olivier Michallat](https://github.com/olim7t), [Hsu-Kwang Hwang](https://github.com/hsuhwang) derived from [here](https://github.com/datastax/java-driver/tree/4.x/examples/src/main/java/com/datastax/oss/driver/examples/concurrent)
## Objectives

* How to limit concurrent query executions with Cassandra in blocking and non-blocking ways. 
* How to throttle Concurrent Requests.

## Project Layout
The following are the main Java files used by the application.
 
* [LimitConcurrencyCustom](/src/main/java/com/datastax/examples/LimitConcurrencyCustom.java) - This example shows executing concurrent queries in blocking ways. The basic java concurrent mechanism such as SEMAPHORE, and Latch are used to control the flow. 
* [LimitConcurrencyCustomAsync](/src/main/java/com/datastax/examples/LimitConcurrencyCustomAsync.java) - This example shows executing concurrent queries in non-blocking ways. Java CompletableFuture is used to achieve non-blocking.   
* [LimitConcurrencyRequestThrottler](/src/main/java/com/datastax/examples/LimitConcurrencyRequestThrottler.java) - Throttling allows you to limit how many requests a session can execute concurrently. This is useful if you have multiple applications connecting to the same Cassandra cluster, and want to enforce some kind of SLA to ensure fair resource allocation.

## Setup and Running

### Prerequisites

* Maven 3 installed on the machine running the examples 
* JDK 8 
* An Apache Cassandra(R) cluster is running and accessible through the contacts points and data center identified in [application.conf](/src/main/resources/application.conf)

### Running
#### Building
At the project root level

```mvn clean package```

This builds the JAR file located at target/java-concurrent-execution-1.0.jar

#### Configuration changes
The main configuration file is `src/main/resources/application.conf`
Change `basic.contact-points` = ["127.0.0.1:9042"] to the IP and cql port of your Cassandra cluster.
Change `basic.local-datacenter` = SearchAnalytics to the Cassandra cluster you want to connect to.

##### Throttling
For throttling the configurations are in the following block

```
advanced.throttler {
    class = ConcurrencyLimitingRequestThrottler
    max-concurrent-requests = 32
    max-queue-size = 10000
}
```

You are encouraged to experiment the configuration. For more details about throttling please check the 
following [link](https://docs.datastax.com/en/developer/java-driver/4.3/manual/core/throttling/)


#### Run the program
* java -cp target/java-concurrent-execution-1.0.jar com.datastax.examples.LimitConcurrencyCustom
* java -cp target/java-concurrent-execution-1.0.jar com.datastax.examples.LimitConcurrencyCustomAsync 
* java -cp target/java-concurrent-execution-1.0.jar com.datastax.examples.LimitConcurrencyRequestThrottler