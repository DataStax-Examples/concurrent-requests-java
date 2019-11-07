# java-concurrent-execution
There three java examples in the repository. Each example shows different way of concurrent query executions. User can learn how to do concurrent executions in blocking and non-blocking ways. One example also shows throttling on concurrent executions. 


## Objectives
To demonstrate 
* how to do concurrent query executions with Cassandra in blocking and non-blocking ways. 
* how to throttle Concurrent Requests.

  
## Project Layout
The java source files are in src/main/java.
The following are the main java files
 
* LimitConcurrencyCustom.java - This example shows executing concurrent queries in blocking ways. The basic java concurrent mechanism such as SEMAPHORE, and Latch are used to control the flow. 
* LimitConcurrencyCustomAsync.java - This example shows executing concurrent queries in non-blocking ways. Java CompletableFuture is used to achieve non-blocking.   
* LimitConcurrencyRequestThrottler Throttling allows you to limit how many requests a session can execute concurrently. This is useful if you have multiple applications connecting to the same Cassandra cluster, and want to enforce some kind of SLA to ensure fair resource allocation.


## Setup and Running

### Prerequisites
* A running Cassandra Cluster
* Maven 3 installed on the machine running the examples 
* JDK 8 

### Setup
Clone this repo locally. 

### Running
#### Building
At the project root level
mvn clean package
This should build target/java-concurrent-execution-1.0.jar

#### Configuration changes
The main configuraiton file is src/main/resources/application.conf
Change basic.contact-points = ["127.0.0.1:9042"] to the IP and cql port of your Cassandra cluster.
Change basic.local-datacenter = SearchAnalytics to the Cassandra cluster you want to connect to.

##### Throttling
For throttling the configurations are in the following block

advanced.throttler {
    class = ConcurrencyLimitingRequestThrottler
    max-concurrent-requests = 32
    max-queue-size = 10000
}

you are encouraged to experiment the configuration around. For more details about throttling please check the following link
https://docs.datastax.com/en/developer/java-driver/4.3/manual/core/throttling/



#### Run the program
* java -cp target/java-concurrent-execution-1.0.jar com.datastax.oss.driver.examples.concurrent.LimitConcurrencyCustom
* java -cp target/java-concurrent-execution-1.0.jar com.datastax.oss.driver.examples.concurrent.LimitConcurrencyCustomAsync 
* java -cp target/java-concurrent-execution-1.0.jar com.datastax.oss.driver.examples.concurrent.LimitConcurrencyRequestThrottler