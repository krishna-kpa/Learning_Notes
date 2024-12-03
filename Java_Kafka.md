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
<br>
![image](https://github.com/user-attachments/assets/128b0149-d9d7-41fe-a5e1-bbaccecf0ba2)





















