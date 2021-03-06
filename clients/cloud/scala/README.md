# Scala Producer and Consumer for Confluent Cloud

Produce messages to and consume messages from a Kafka cluster using the Scala Producer and Consumer, and Kafka Streams API.
For more information, please see the [application development documentation](https://docs.confluent.io/current/api-javadoc.html?utm_source=github&utm_medium=demo&utm_campaign=ch.examples_type.community_content.clients-ccloud)

# Prerequisites

To run this example, create a local file with configuration parameters to connect to your Kafka cluster, which can be on your local host, [Confluent Cloud](https://www.confluent.io/confluent-cloud/?utm_source=github&utm_medium=demo&utm_campaign=ch.examples_type.community_content.clients-ccloud), or any other cluster.
If this is a Confluent Cloud cluster, you must have:

* Access to a [Confluent Cloud](https://www.confluent.io/confluent-cloud/?utm_source=github&utm_medium=demo&utm_campaign=ch.examples_type.community_content.clients-ccloud) cluster
* Initialize a properties file at `$HOME/.ccloud/config` with configuration to your Confluent Cloud cluster:

```shell
$ cat $HOME/.ccloud/config
bootstrap.servers=<BROKER ENDPOINT>
ssl.endpoint.identification.algorithm=https
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username\="<API KEY>" password\="<API SECRET>";
```

# Quickstart

1. Now we can turn our attention to the client examples in this directory.

	1. First run the example consumer
		```shell
		$ cd examples/clients/cloud/scala
		# Build the client examples
		$ sbt clean compile
		
		# Run the consumer 
		$ sbt "runMain io.confluent.examples.clients.scala.Consumer $HOME/.ccloud/config testtopic"
		```
		You should see
		
		```
		<snipped>
		
		Polling
		....
		<snipped>
		```
	2. Next, in a new window, run the Streams app:
	    ```shell
    		$ cd examples/clients/cloud/scala
    		# Build the client examples
    		$ sbt clean compile
    		
    		# Run the consumer
    		$ sbt "runMain io.confluent.examples.clients.scala.Streams $HOME/.ccloud/config testtopic"
        ```

	3. Then, in a new window run the Kafka producer application to write records to the Kafka cluster, you should see these appear in the consumer window.

		```shell
		$ sbt "runMain io.confluent.examples.clients.scala.Producer $HOME/.ccloud/config testtopic"
		```
        		
        You should see
        		
        ```
        <snipped>
        Produced record at testtopic-0@120
        Produced record at testtopic-0@121
        Produced record at testtopic-0@122
        Produced record at testtopic-0@123
        Produced record at testtopic-0@124
        Produced record at testtopic-0@125
        Produced record at testtopic-0@126
        Produced record at testtopic-0@127
        Produced record at testtopic-0@128
        Produced record at testtopic-0@129
        Wrote ten records to testtopic
        [success] Total time: 6 s, completed 10-Dec-2018 16:50:13

		```
		
		In the consumer window you should see:
		```
		<snipped>
	    Polling
        Consumed record with key alice and value {"count":1}, and updated total count to 1
        Consumed record with key alice and value {"count":2}, and updated total count to 3
        Consumed record with key alice and value {"count":3}, and updated total count to 6
        Consumed record with key alice and value {"count":4}, and updated total count to 10
        Consumed record with key alice and value {"count":5}, and updated total count to 15
        Consumed record with key alice and value {"count":6}, and updated total count to 21
        Consumed record with key alice and value {"count":7}, and updated total count to 28
        Consumed record with key alice and value {"count":8}, and updated total count to 36
        Consumed record with key alice and value {"count":9}, and updated total count to 45
        Consumed record with key alice and value {"count":10}, and updated total count to 55
        Polling

```
        In the Streams window you should see:
        ```
        [Consumed record]: alice, 1
        [Consumed record]: alice, 2
        [Consumed record]: alice, 3
        [Consumed record]: alice, 4
        [Consumed record]: alice, 5
        [Consumed record]: alice, 6
        [Consumed record]: alice, 7
        [Consumed record]: alice, 8
        [Consumed record]: alice, 9
        [Consumed record]: alice, 10
        [Running count]: alice, 1
        [Running count]: alice, 3
        [Running count]: alice, 6
        [Running count]: alice, 10
        [Running count]: alice, 15
        [Running count]: alice, 21
        [Running count]: alice, 28
        [Running count]: alice, 36
        [Running count]: alice, 45
        [Running count]: alice, 55
   ```
Hit Ctrl+C in both windows to stop the Consumer and Streams
