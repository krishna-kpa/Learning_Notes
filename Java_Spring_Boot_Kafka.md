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
<br>
[Apache+Kafka+Topic_+Summary+document..pdf](https://github.com/user-attachments/files/18006724/Apache%2BKafka%2BTopic_%2BSummary%2Bdocument.pdf)
<br>
[Kafka+CLI_+Producer+Summary.pdf](https://github.com/user-attachments/files/18006731/Kafka%2BCLI_%2BProducer%2BSummary.pdf)
<br>
[Kafka+CLI_+Consumer+Summary.pdf](https://github.com/user-attachments/files/18006730/Kafka%2BCLI_%2BConsumer%2BSummary.pdf)
<br>
