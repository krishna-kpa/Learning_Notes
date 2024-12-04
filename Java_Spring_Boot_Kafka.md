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


# Kafka Topics and Partitions

## What is an Apache Kafka Topic?

A Kafka Topic is a logical collection of messages that are produced and consumed by Kafka applications. A message is a piece of data that contains some information, such as an event, a record, or a notification. For example, a message could be a product-created event, which contains the details of a new product added to an online store.

A Kafka Topic has a unique name that identifies it in a Kafka cluster. A Kafka cluster is a group of servers, called brokers, that store and manage the topics and messages. A Kafka application can connect to a Kafka cluster and interact with the topics and messages.

## What is Topic Partition?

A Kafka Topic is divided into one or more partitions, each of which stores a subset of messages in an ordered sequence. A partition is a physical unit of storage and processing in a Kafka broker. Each partition has a unique identifier, called a partition ID, that is assigned by the broker.

The number of partitions for a topic is determined when the topic is created, and it can be changed later. The number of partitions affects the scalability, parallelism, fault-tolerance, and ordering guarantees of a topic.

### Impact of Partitions:
- **Scalability**: The more partitions a topic has, the more messages it can store and handle. A topic with many partitions can be distributed across multiple brokers, which increases the storage capacity and performance of the topic.
  
- **Parallelism**: The more partitions a topic has, the more producers and consumers can interact with it concurrently. A topic with many partitions can have multiple producers sending messages to different partitions, and multiple consumers receiving messages from different partitions. This increases the throughput and efficiency of the topic.

- **Fault-tolerance**: The more partitions a topic has, the more resilient it is to failures. A topic with many partitions can have replicas, which are copies of the partitions stored on different brokers. If a broker fails, the replicas can take over and continue serving the messages. This increases the availability and reliability of the topic.

- **Ordering guarantees**: The more partitions a topic has, the less strict the ordering guarantees are. A topic with many partitions can only guarantee the order of messages within each partition, but not across partitions. This means that messages from different partitions may be delivered out of order to the consumers. This may or may not be acceptable, depending on the use case of the topic.

## How to Create a Topic?

To create a topic in a Kafka cluster, we can use the `kafka-topics.sh` command-line tool, which is provided by Kafka. This tool allows us to perform various operations on topics, such as creating, deleting, listing, and describing topics.

### Parameters to Create a Topic:
- `--bootstrap-server`: The address of one or more brokers in the Kafka cluster that we want to connect to. For example, `localhost:9092`.
- `--create`: The flag that indicates that we want to create a topic.
- `--topic`: The name of the topic that we want to create. For example, `product-created-events-topic`.
- `--partitions`: The number of partitions that we want to create for the topic. For example, `3`.
- `--replication-factor`: The number of replicas that we want to create for each partition of the topic. For example, `2`.

### Example Command:

```bash
kafka-topics.sh --bootstrap-server localhost:9092 --create --topic product-created-events-topic --partitions 3 --replication-factor 2
