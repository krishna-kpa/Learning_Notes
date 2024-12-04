## Learn to build and test event-driven Spring Boot Microservices using Apache Kafka

#### Installing Kafka on Windows
1. Install Docker
2. Create Docker Compose (yml)
```
version: "3.8"
services:
  kafka-1:
    image: bitnami/kafka:latest
    ports:
      - "9092:9092"
    environment:
      - KAFKA_CFG_NODE_ID=1
      - KAFKA_KRAFT_CLUSTER_ID=KRISHNAPRASAD
      - KAFKA_CFG_PROCESS_ROLES=controller,broker
      - KAFKA_CFG_CONTROLLER_QUORUM_VOTERS=1@kafka-1:9091
      - KAFKA_CFG_LISTENERS=PLAINTEXT://:9090,CONTROLLER://:9091,EXTERNAL://:9092
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://kafka-1:9090,EXTERNAL://${HOSTNAME:-localhost}:9092
      - KAFKA_CFG_SECURITY_PROTOCOL_MAP=CONTROLLER:PLAINTEXT,EXTERNAL:PLAINTEXT,PLAINTEXT:PLAINTEXT
      - KAFKA_CFG_CONTROLLER_LISTENER_NAMES=CONTROLLER
      - KAFKA_CFG_INTER_BROKER_LISTENER_NAME=PLAINTEXT
    volumes:
      - path/where/to/store/server-1:/bitnami/kafka
```
add HOSTNAME  in the env file. 

What is an Apache
Kafka Topic?
A Kafka Topic is a logical collection of messages that are produced and
consumed by Kafka applications. A message is a piece of data that
contains some information, such as an event, a record, or a notificatio
For example, a message could be a product created event, which
contains the details of a new product that was added to an online store.
A Kafka Topic has a unique name that identifies it in a Kafka cluster.
Kafka cluster is a group of servers, called brokers, that store and manage
the topics and messages. A Kafka application can connect to a Kafka
cluster and interact with the topics and messages.
What is Topic Partition?
A Kafka Topic is divided into one or more partitions, each of which
stores a subset of messages in an ordered sequence. A partition is a
physical unit of storage and processing in a Kafka broker. Each partition
has a unique identifier, called a partition ID, that is assigned by t
broker.
The number of partitions for a topic is determined when the topic is
created, and it can be changed later. The number of partitions affects the
scalability, parallelism, fault-tolerance, and ordering guarantees of a
topic.
● Scalability: The more partitions a topic has, the more messages it
can store and handle. A topic with many partitions can be
distributed across multiple brokers, which increases the storage
capacity and performance of the topic.
● Parallelism: The more partitions a topic has, the more producers
and consumers can interact with it concurrently. A topic with many
partitions can have multiple producers sending messages to
different partitions, and multiple consumers receiving messages
from different partitions. This increases the throughput and
efficiency of the top
● Fault-tolerance: The more partitions a topic has, the more resilient
it is to failures. A topic with many partitions can have replicas,
which are copies of the partitions stored on different brokers. If a
broker fails, the replicas can take over and continue serving the
messages. This increases the availability and reliability of the topic.
● Ordering guarantees: The more partitions a topic has, the less
strict the ordering guarantees are. A topic with many partitions can
only guarantee the order of messages within each partition, but not
across partitions. This means that messages from different
partitions may be delivered out of order to the consumers. This
may or may not be acceptable, depending on the use case of the
topic.
How to create topic?
To create a topic in a Kafka cluster, we can use the kafka-topics.sh
command line tool, which is provided by Kafka. This tool allows us to
perform various operations on topics, such as creating, deleting, listing,
and describing topics.
To create a topic, we need to specify the following parameters:
● --bootstrap-server: The address of one or more brokers in the
Kafka cluster that we want to connect to. For example,
localhost:9092.
● --create: The flag that indicates that we want to create a topi
● --topic: The name of the topic that we want to create. For
example, product-created-events-topic.
● --partitions: The number of partitions that we want to create
for the topic. For example, 3.
● --replication-factor: The number of replicas that we want to
create for each partition of the topic. For example, 2.
For example, the following command creates a topic named
product-created-events-topic with 3 partitions and 2 replicas in a
Kafka cluster with a broker at localhost:9092:
kafka-topics.sh --bootstrap-server localhost:9092 --create --topic
product-created-events-topic --partitions 3 --replication-factor 2
If the topic is created successfully, the tool will print a confirmati
message, such as:
Created topic product-created-events-topic.
If the topic already exists, or if there is an error, the tool will print an error
message, such as:
Topic 'product-created-events-topic' already exists.
How to create topic with
partitions?
As I explained in the previous section, a Kafka Topic is divided into one
or more partitions, each of which stores a subset of messages in an
ordered sequence. The number of partitions for a topic affects its
scalability, parallelism, fault-tolerance, and ordering guarantees.
To create a topic with partitions, you can use the kafka-topics.sh
command line tool, which I introduced in the previous section. This tool
allows you to perform various operations on topics, such as creating,
deleting, listing, and describing topics.
To create a topic with partitions, you need to specify the following
parameters:
● --bootstrap-server: The address of one or more brokers in the
Kafka cluster that you want to connect to. For example,
localhost:9092.
● --create: The flag that indicates that you want to create a topi
● --topic: The name of the topic that you want to create. For
example, product-created-events-topic.
● --partitions: The number of partitions that you want to create
for the topic. For example, 3.
For example, the following command creates a topic named
product-created-events-topic with 3 partitions in a Kafka cluster
with a broker at localhost:9092:
kafka-topics.sh --bootstrap-server localhost:9092 --create --topic
product-created-events-topic --partitions 3
If the topic is created successfully, the tool will print a confirmati
message, such as:
Created topic product-created-events-topic.
If the topic already exists, or if there is an error, the tool will print an error
message, such as:
Topic 'product-created-events-topic' already exists.
How to update topic?
Sometimes, you may want to change the configuration and properties 
an existing topic, such as changing the number of partitions, the
replication factor, the retention policy, and other parameters. You can do
this by using the kafka-topics.sh command line tool, which I
introduced in the previous sections. This tool allows you to perform
various operations on topics, such as creating, deleting, listing, and
describing topics.
To update a topic, you need to specify the following parameters:
● --bootstrap-server: The address of one or more brokers in the
Kafka cluster that you want to connect to. For example,
localhost:9092.
● --alter: The flag that indicates that you want to update a topi
● --topic: The name of the topic that you want to update. For
example, product-created-events-topic.
● --config: A comma-separated list of key=value pairs, where
key is the name of the parameter that you want to update, and
value is the new value that you want to assign to it. For example,
retention.ms=43200000.
For example, the following command updates the retention policy of the
topic named product-created-events-topic to 12 hours in a Kafka
cluster with a broker at localhost:9092:
kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic
product-created-events-topic --config retention.ms=43200000
There are many parameters that you can update for a topic, such as the
number of partitions, the replication factor, the cleanup policy, the
compression type, and more. You can find the full list of top
configurations and their descriptions in the Kafka topic configura
reference.
In the following subsections, I will show you how to update some of the
common parameters for a topic.
How to update topic partitions?
The number of partitions for a topic determines how many subsets of
messages the topic is divided into. The number of partitions affects the
scalability, parallelism, fault-tolerance, and ordering guarantees of a
topic.
To update the number of partitions for a topic, you can use the
--partitions option in the kafka-topics.sh command line tool,
and specify the new number of partitions that you want to assign to the
topic. For example, the following command increases the number of
partitions for the topic named product-created-events-topic to 6
in a Kafka cluster with a broker at localhost:9092:
kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic
product-created-events-topic --partitions 6
Note that you can only increase the number of partitions for a topic, but
not decrease it. This is because reducing the number of partitions would
cause data loss and inconsistency. If you want to reduce the number of
partitions for a topic, you have to delete the topic and recreate it with the
desired number of partitions.
How to update topic replication factor?
The replication factor for a topic determines how many copies of each
partition are stored on different brokers. The replication factor affects
the fault-tolerance and availability of a topic.
To update the replication factor for a topic, you can use the
--replication-factor option in the kafka-topics.sh command
line tool, and specify the new replication factor that you want to assign
to the topic. For example, the following command increases the
replication factor for the topic named
product-created-events-topic to 3 in a Kafka cluster with a
broker at localhost:9092:
kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic
product-created-events-topic --replication-factor 3
Note that you can only increase the replication factor for a topic, but not
decrease it. This is because reducing the replication factor would cause
data loss and inconsistency. If you want to reduce the replication factor
for a topic, you have to delete the topic and recreate it with the desired
replication factor.
How to update retention policy?
The retention policy for a topic determines how long the messages in the
topic are retained before they are deleted. The retention policy affects
the storage capacity and performance of a topic.
To update the retention policy for a topic, you can use the --config
option in the kafka-topics.sh command line tool, and specify the
new retention policy that you want to assign to the topic. There are two
ways to specify the retention policy for a topic: by time or by size.
● To specify the retention policy by time, you can use the
retention.ms parameter, and specify the maximum time in
milliseconds that a message can remain in the topic before it is
deleted. For example, the following command sets the retention
policy for the topic named product-created-events-topic to
12 hours in a Kafka cluster with a broker at localhost:9092:
● kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic
product-created-events-topic --config retention.ms=43200000
● To specify the retention policy by size, you can use the
retention.bytes parameter, and specify the maximum size in
bytes that the topic can occupy before the oldest messages are
deleted. For example, the following command sets the retention
policy for the topic named product-created-events-topic to
1 GB in a Kafka cluster with a broker at localhost:9092:
● kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic
product-created-events-topic --config retention.bytes=1073741824
You can also use both parameters together, and the topic will be deleted
when either of the conditions is met.
How to delete topic?
Sometimes, you may want to delete a topic from a Kafka cluster, either
because you no longer need it, or because you want to recreate it with
different settings. You can do this by using the kafka-topics.sh
command line tool, which I introduced in the previous sections. This tool
allows you to perform various operations on topics, such as creating,
updating, listing, and describing topics.
To delete a topic, you need to specify the following parameters:
● --bootstrap-server: The address of one or more brokers in the
Kafka cluster that you want to connect to. For example,
localhost:9092.
● --delete: The flag that indicates that you want to delete a topi
● --topic: The name of the topic that you want to delete. For
example, product-created-events-topic.
For example, the following command deletes the topic named
product-created-events-topic from a Kafka cluster with a broker
at localhost:9092:
kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic
product-created-events-topic
If the topic is deleted successfully, the tool will print a confirmati
message, such as:
Topic product-created-events-topic is marked for deletion.
If the topic does not exist, or if there is an error, the tool will print an error
message, such as:
Topic 'product-created-events-topic' does not exist.
How to read messages from the topic?
A Kafka Consumer is an application that receives messages from one or
more topics in a Kafka cluster. A consumer can subscribe to one or more
topics, and consume messages from them in a streaming or batch
manner. A consumer can also specify the offset, or the position, from
which it wants to start consuming messages from a topic. For example,
a consumer can start from the beginning, the end, or a specific offset 
a topic.
To read messages from a topic, you can use the
kafka-console-consumer.sh command line tool, which I introduced
in the previous sections. This tool allows you to consume messages
from a topic and print them to the console (standard output). By default,
it outputs the raw bytes in the message with no formatting (using the
Default Formatter).
To read messages from a topic, you need to specify the following
parameters:
● --bootstrap-server: The address of one or more brokers in the
Kafka cluster that you want to connect to. For example,
localhost:9092.
● --topic: The name of the topic that you want to read messages
from. For example, product-created-events-topic.
● --from-beginning: The flag that indicates that you want to re
messages from the beginning of the topic. If you omit this flag, t
tool will read messages from the end of the topic, or the latest
offset.
● --property: A comma-separated list of key=value pairs, where
key is the name of the property that you want to set, and value is
the value that you want to assign to it. For example,
print.key=true,key.separator=-. You can use this option to
customize the output format of the messages, such as showing
the key and the value, or changing the separator between them.
For example, the following command reads messages from the topic
named product-created-events-topic from the beginning of the
topic, and shows the key and the value of each message, separated by a
dash, in a Kafka cluster with a broker at localhost:9092:
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic
product-created-events-topic --from-beginning --property
print.key=true,key.separator=-
If the tool successfully connects to the topic and starts consuming
messages, it will print them to the console.


How to Produce Messages to Kafka Topics
using Apache Kafka CLI
Apache Kafka is a distributed streaming platform that allows you to publish and
subscribe to streams of data. In this tutorial, you will learn how to use the Apache Kafka
command-line interface (CLI) to produce messages to Kafka topics.
A Kafka topic is a logical name for a stream of data. You can create multiple topics in
Kafka, and each topic can have multiple partitions. Partitions are the units of parallelism
in Kafka, and they allow you to scale up your data processing.
How to produce messages to Kafka topics without a key
To produce messages to Kafka topics without a key, you will use the
kafka-console-producer.sh script that comes with the Kafka installation. This
script allows you to send messages to a Kafka topic from the standard input (stdin).
Before you run the script, you need to make sure that you have a running Kafka cluster.
In this tutorial, I will assume that you have a Kafka cluster running on
localhost:9092. You can change this to match your cluster configuratio
To produce messages to a Kafka topic called product-created-events-topic,
you can run the following command in a terminal:
./kafka-console-producer.sh --bootstrap-server localhost:9092
--topic product-created-events-topic
The --bootstrap-server option specifies the address of the Kafka broker that t
producer will connect to. The --topic option specifies the name of the topic that t
producer will send messages to.
After you run the command, you will see a > prompt, indicating that the producer is
ready to accept messages from stdin. You can type any message you want, and press
enter to send it to the topic. For example:
> Hello, world!
> This is a test message.
> I am learning how to use Kafka CLI.
Each line that you type will be sent as a separate message to the topic. You can verify
that the messages are received by the topic by using another terminal to run the
kafka-console-consumer.sh script, which allows you to consume messages from
a Kafka topic. For example:
./kafka-console-consumer.sh --bootstrap-server localhost:9092
--topic product-created-events-topic --from-beginning
The --from-beginning option tells the consumer to read all the messages from the
topic, starting from the earliest one. You should see the messages that you typed in the
producer terminal, in the same order. For example:
Hello, world!
This is a test message.
I am learning how to use Kafka CLI.
To stop the producer or the consumer, you can press Ctrl+C in the terminal.
How to produce messages to Kafka topics with a key
Messages in Kafka topics are stored as a key-value pair. If you send a message without
a key, then the key is null. However, sometimes you may want to send a message with a
key, for example, to ensure that messages with the same key are stored in the same
partition, and thus preserve the order of the messages.
To produce messages to Kafka topics with a key, you will use the same
kafka-console-producer.sh script, but with some additional properties. The fir
property is parse.key=true, which tells the producer to expect each message to be in
the format of key:value. The second property is key.separator=: (or any other
character you prefer), which tells the producer what character to use to separate the key
and the value in each message.
To produce messages with a key to the same topic
product-created-events-topic, you can run the following command in a
terminal:
./kafka-console-producer.sh --bootstrap-server localhost:9092
--topic product-created-events-topic --property "parse.key=true"
--property "key.separator=:"
After you run the command, you will see a > prompt, indicating that the producer is
ready to accept messages from stdin. You can type any message you want, as long as it
follows the key:value format, and press enter to send it to the topic. For example:
> product_id:123
> product_name:Kafka mug
> product_price:9.99
Each line that you type will be sent as a separate message to the topic, with the key and
the value separated by the : character. You can verify that the messages are received by
the topic by using another terminal to run the kafka-console-consumer.sh script,
with the same properties as the producer. For example:
./kafka-console-consumer.sh --bootstrap-server localhost:9092
--topic product-created-events-topic --from-beginning --property
"print.key=true" --property "key.separator=:"
The print.key=true property tells the consumer to print both the key and the value
of each message, separated by the key.separator character. You should see the
messages that you typed in the producer terminal, in the same order. For example:
product_id:123
product_name:Kafka mug
product_price:9.99
To stop the producer or the consumer, you can press Ctrl+C in the terminal.

Consuming messages from a Kafka topic
from the beginning
In this section, you will learn how to consume messages from a Kafka topic using the
kafka-console-consumer script. This script allows you to read messages from a
topic and display them on the terminal.
Prerequisites
Before you start, you need to have the following:
● Apache Kafka installed on your machine.
● A Kafka topic with some messages in it. You can create a topic and produce
some messages using the kafka-topics and kafka-console-producer
scripts, as explained in the previous lessons.
● A terminal window open and ready to run commands.
Steps
To consume messages from a Kafka topic, follow these steps:
1. Navigate to the Kafka folder where you installed Kafka. For example, if you
installed Kafka in /Users/sergeykargopolov/kafka, run this command:
cd /Users/sergeykargopolov/kafka
2. Navigate to the bin folder where the Kafka CLI scripts are located. Run this
command:
cd bin
3. To consume messages from a Kafka topic, run the kafka-console-consumer
script with the following parameters:
● --topic: The name of the topic you want to read messages from. For example,
my-topic.
● --from-beginning: A flag that tells the consumer to read all messages fr
the topic, starting from the first one. If you omit this flag, the consumer will 
read new messages that arrive after you start the script.
● --bootstrap-server: The address of one or more Kafka servers that you
want to connect to. For example, localhost:9092.
For example, to consume messages from a topic called my-topic from the beginning,
run this command:
bin/kafka-console-consumer.sh --topic my-topic --from-beginning
--bootstrap-server localhost:9092
4. Press Enter to run the command. You should see the messages from the topic
displayed on the terminal, one per line. For example:
Hello world
Hello Kafka
Hello world 2
Hello world 3
Hello world 4
Notice that the script does not exit after reading all the messages. It keeps running and
waiting for more messages to arrive. As soon as a new message is sent to the topic, the
consumer will read it and display it on the terminal.
You can start more consumers and run the same command to consume messages
from the same topic.
Each consumer will read all messages from the topic, regardless of whether they have
been consumed by other consumers or not. This is because Kafka does not delete
messages from a topic after they are consumed. It keeps them in the topic for a
configurable period of time, so that other consumers can read them as wel
To stop the consumer script, press Ctrl+C on the terminal. This will terminate the
script and disconnect from the Kafka cluster.
You’re welcome. I’m glad to help you with your tutorial. 😊
Here is the new section that I created based on the text you provided. I tried to keep it
brief and concise as a summary.
Consuming new messages only
In this section, you will learn how to consume only new messages from a Kafka topic
using the kafka-console-consumer script. This means that you will not read any
messages that were sent to the topic before you start the script.
To consume only new messages from a Kafka topic, follow these steps:
1. Open a terminal window and navigate to the bin folder where the Kafka CLI
scripts are located.
2. Run the kafka-console-consumer script with the following parameters:
● --topic: The name of the topic you want to read messages from. For example,
my-topic.
● --bootstrap-server: The address of one or more Kafka servers that you
want to connect to. For example, localhost:9092.
For example, to consume only new messages from a topic called my-topic, run this
command:
bin/kafka-console-consumer.sh --topic my-topic
--bootstrap-server localhost:9092
Consuming Key:Value Pair Messages from a
Kafka topic
In this section, you will learn how to consume messages that are sent as key:value pairs
from a Kafka topic. A key:value pair is a message that has two parts: a key and a value,
separated by a delimiter.
To consume key:value pair messages, you need to run the kafka-console-consumer
script with the same parameters as for regular messages, plus two optional parameters
to print the key and the value:
● --property print.key=true: To print the key part of the message.
● --property print.value=true: To print the value part of the message.
For example, to consume key:value pair messages from a topic called my-topic from
the beginning and print both the key and the value, run this command:
bin/kafka-console-consumer.sh --topic my-topic --from-beginning
--bootstrap-server localhost:9092 --property print.key=true
--property print.value=true
By default, the consumer will only print the value part of the message, not the key. You
can change this by setting the print.key and print.value properties to true or
false as you wish.
The key:value pair messages are stored in the topic with the delimiter that you specifi
when you produced them. For example, if you used a colon (:) as the delimiter, the
messages will be stored as key:value in the topic. The consumer will display the key
and the value separated by a tab on the terminal. For example:
firstName Sergey
lastName Kargopolov
You can consume key:value pair messages from any topic that you create or subscribe
to, as long as the producer sends them in the right format.
Consuming messages in order
In this section, you will learn how to store and consume messages in Kafka topic, so
that they are read in the same order they were sent. This is useful when you want to
preserve the sequence of events or transactions in your messages.
To store messages in order, you need to use the same key for all the messages that
belong to the same sequence. The key can be any string that identifies or groups t
messages, such as a user ID or a product ID. Kafka will use the key to determine which
partition to store the message in. Messages with the same key will be stored in the
same partition and will be read in order.
To produce messages with a key, run the kafka-console-producer script with the
following parameters:
● --topic: The name of the topic you want to send messages to. For example,
messages-order.
● --bootstrap-server: The address of one or more Kafka servers that you
want to connect to. For example, localhost:9092.
● --property parse.key=true: A flag that tells the producer to enable t
key:value pair support.
● --property key.separator=:: A parameter that specifies the separat
between the key and the value. You can use any character as the separator, but
make sure it is not part of the key or the value.
For example, to produce messages with a key and a colon (:) as the separator to a topic
called messages-order, run this command:
./kafka-console-producer.sh --bootstrap-server localhost:9092
--topic messages-order --property parse.key=true --property
key.separator=:
Press Enter to run the command. You are ready to start sending messages. Type a
message in the format key:value and press Enter to send it. For example, to send a
message with the key 1 and the value First message, type this:
1:First message
Send more messages with the same key. For example:
1:Second message
1:Third message
1:Fourth message
1:Fifth message
1:Sixth message
Notice that you are using the same key for all the messages. This means that they will
be stored in the same partition and will be read in order.
To test this, open another terminal window and run the kafka-console-consumer
script to consume messages from the same topic. For example, run this command:
./kafka-console-consumer.sh --topic messages-order
--bootstrap-server localhost:9092 --from-beginning --property
print.key=true --property print.value=true
Press Enter to run the command. You should see the messages from the topic
displayed on the terminal, one per line, with the key and the value separated by a tab. For
example:
1 First message
1 Second message
1 Third message
1 Fourth message
1 Fifth message
1 Sixth message
Notice that the messages are displayed in the same order they were sent. This is
because they have the same key and are stored in the same partition.
To stop the producer or the consumer script, press Ctrl+C on the terminal. This will
terminate the script and disconnect from the Kafka cluster.
