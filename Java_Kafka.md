### What is Event Streaming ?
 - is the digital equivalent of the human body's central nervous system.
 - capturing data in real-time from event sources like db, sensors, mobiles etc and storing them for later retrieval.
### What can i use event streaming for ?
 - To track and monitor cars, trucks, shipments etc.
 - To process payments and financial transactions in real time.
 - To continuously capture and analyze sensor data from IoT devices or other equipments.
### Apache Kafka® is an event streaming platform. What does that mean?
 - kafka combines thre key capabilities :
    - To publish and subscribe to streams of events.
    - To store streams of events  durably and reliably for as long as you want.
    - To process streams of events as they occur or retrospectively.
### How does Kafka work in a nutshell?
 - It can be deployed on bare-metal hardware, virtual machines, and containers in on-premise as well as cloud environments
      - Servers :
        - Kafka is run as a cluster of one or more servers that can span multiple datacenters or cloud regions.
        - Some of these servers form the storage layer, called the brokers.
        - Other servers run kafka connect to continuosly import and export data as event streams to integrate kafka with your systems.
      - Clients :
        - They allow to write distributed applications and microservices that read, write and process streams of events in parallel.
## Main Concepts and Terminology

#### Event
An event records the fact that something happened in the world or in your business.
When you read or write data to kafka, you do this in the form of events.
An event has a key, value, timestamp, and optional metadata headers.

#### Producers and Consumers
are those client applications that publish events to kafka, and consumers are those that subscibe to these events.
In kafka producers and consumers are fully decoupled and agnostic of each other.
Producers never need to wait for Consumers.

#### Topic and Partitions
Events are oranized and durably stored in topics. 
Similar to folder in the file system.
Topics in kafka are always multi-producer and multi-subscriber. (zero or many)
Events are not deleted after consumption (We can define life span of the events).

Topics are partiotioned, meaning a topic is spread over a number of buckets located on different kafka brokers. This distributed placement of your data is very important for scalability because it allows clients to both read and write the data from/to many brokers at the same time.
When an event published to a topic it is appended to one of the partition.
Events with the same key are written to the same partition.
Every topic can be replicated, even across geo-regions or datacenters.
<br>
![image](https://github.com/user-attachments/assets/128b0149-d9d7-41fe-a5e1-bbaccecf0ba2)

## Kafka APIs
- The Admin API to manage and inspect topics, brokers, and other Kafka objects.
- The Producer API to publish (write) a stream of events to one or more Kafka topics.
- The Consumer API to subscribe to (read) one or more topics and to process the stream of events produced to them.
- The Kafka Streams API to implement stream processing applications and microservices. It provides higher-level functions to process event streams, including transformations, stateful operations like aggregations and joins, windowing, processing based on event-time, and more. Input is read from one or more topics in order to generate output to one or more topics, effectively transforming the input streams to output streams.
- The Kafka Connect API to build and run reusable data import/export connectors that consume (read) or produce (write) streams of events from and to external systems and applications so they can integrate with Kafka. For example, a connector to a relational database like PostgreSQL might capture every change to a set of tables. However, in practice, you typically don't need to implement your own connectors because the Kafka community already provides hundreds of ready-to-use connectors.


















