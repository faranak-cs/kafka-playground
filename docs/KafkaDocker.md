# Set Up Kafka using Docker

> Do not run the commands directly with `docker exec`. In that case, the "docker exec" would be terminated but the `kafka-topics` would continue running in the container.

## Spin up broker and zookeeper

```
docker-compose up
```

## Producer and Consume the Messages

- Create a Kafka topic using the **kafka-topics** command.

```
docker exec -it kafka1 bash
```

```
kafka-topics --bootstrap-server kafka1:19092 \
             --create \
             --topic test-topic \
             --replication-factor 1 --partitions 1
```

- Produce Messages to the topic.

```
docker exec -it kafka1 bash
```

```
kafka-console-producer --bootstrap-server kafka1:19092 \
                       --topic test-topic
```

- Consume Messages from the topic.

```
docker exec -it kafka1 bash
```

```
kafka-console-consumer --bootstrap-server kafka1:19092 \
                       --topic test-topic \
                       --from-beginning
```

## Producer and Consume the Messages With Key and Value

- Produce Messages with Key and Value to the topic.

```
docker exec -it kafka1 bash
```

```
kafka-console-producer --bootstrap-server kafka1:19092 \
                       --topic test-topic \
                       --property "key.separator=-" --property "parse.key=true"
```

- Consuming messages with Key and Value from a topic.

```
docker exec -it kafka1 bash
```

```
kafka-console-consumer --bootstrap-server kafka1:19092 \
                       --topic test-topic \
                       --from-beginning \
                       --property "key.separator= - " --property "print.key=true"
```

### Consume Messages using Consumer Groups

```
docker exec -it kafka1 bash
```

```
kafka-console-consumer --bootstrap-server kafka1:19092 \
                       --topic test-topic --group console-consumer-41911\
                       --property "key.separator= - " --property "print.key=true"
```

- Example Messages:

```
a-abc
b-bus
```

### Consume Messages With Headers

```
docker exec -it kafka1 bash
```

```
kafka-console-consumer --bootstrap-server kafka1:19092 \
                       --topic library-events.DLT \
                       --property "print.headers=true" --property "print.timestamp=true"
```

- Example Messages:

```
a-abc
b-bus
```

## Other Kafka Commands

### List the topics in a cluster

```
docker exec -it kafka1 bash
```

```
kafka-topics --bootstrap-server kafka1:19092 --list
```

### Describe topic

- Command to describe all the Kafka topics.

```
docker exec -it kafka1 bash
```

```
kafka-topics --bootstrap-server kafka1:19092 --describe
```

- Command to describe a specific Kafka topic.

```
docker exec -it kafka1 bash
```

```
kafka-topics --bootstrap-server kafka1:19092 --describe \
--topic test-topic
```

### Alter topic Partitions

```
docker exec -it kafka1 bash
```

```
kafka-topics --bootstrap-server kafka1:19092 \
--alter --topic test-topic --partitions 40
```

### How to view consumer groups

```
docker exec -it kafka1 bash
```

```
kafka-consumer-groups --bootstrap-server kafka1:19092 --list
```

#### Consumer Groups and their Offset

```
docker exec -it kafka1 bash
```

```
kafka-consumer-groups --bootstrap-server kafka1:19092 \
--describe --group console-consumer-41911
```

## Log file and related config

- Log into the container.

```
docker exec -it kafka1 bash
```

- The config file is present in the below path.

```
/etc/kafka/server.properties
```

- The log file is present in the below path.

```
/var/lib/kafka/data/
```

### How to view the commit log?

```
docker exec -it kafka1 bash
```

```
kafka-run-class kafka.tools.DumpLogSegments \
--deep-iteration \
--files /var/lib/kafka/data/test-topic-0/00000000000000000000.log

```
