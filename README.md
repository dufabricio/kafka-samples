Video utilizado para os estudos :

https://www.youtube.com/watch?v=M4mHkuQ0mqQ

# Exercise 1 - Create Basic Topic

```
./kafka-topics.sh --create --bootstrap-server localhost:29092 --partitions 3 --replication-factor 1 --topic test-3-1
./kafka-topics.sh --list --bootstrap-server localhost:29092
./kafka-topics.sh --describe --bootstrap-server localhost:29092
```

# Exercise 2 - Create Topic with Partition and Replica

```
./kafka-topics.sh --create --bootstrap-server localhost:29092 --partitions 3 --replication-factor 3 --topic test-3-3
./kafka-topics.sh --list --bootstrap-server localhost:29092
./kafka-topics.sh --describe --bootstrap-server localhost:29092
```

## Create a Producer

```
./kafka-console-producer.sh --broker-list localhost:29092 --topic test-3-3
```

## Create Consumers with Group

```
./kafka-console-consumer.sh --bootstrap-server localhost:29092 --topic test-3-3 --group grupoa
```


```
./kafka-console-consumer.sh --bootstrap-server localhost:29092 --topic test-3-3 --group grupoa
```

## List Group

```
./kafka-consumer-groups.sh --group grupoa --bootstrap-server localhost:29092 --describe
```

# kafka-samples


## Replication Settings

https://www.conduktor.io/kafka/kafka-topic-configuration-min-insync-replicas


## Describe settings for brokers

```
 sudo ./kafka-configs.sh --describe --bootstrap-server localhost:29092  --entity-type brokers
```

## Add settings for brokers

```
sudo kafka-configs.sh --bootstrap-server localhost:29092 --alter --entity-type brokers --entity-default  --add-config min.insync.replicas=2
```

## Delete settings for broker

```
sudo kafka-configs.sh --bootstrap-server localhost:29092 --alter --entity-type brokers --entity-default  --delete-config min.insync.replicas
```

# Simulating Exception by Wrong Configs

For execute commands of kafka client.
```
cd ./kafka/bin
```

**Remember: We have a cluster with min-insync-replicas=2**

## Creating a topic with replication-factor =1 ( less than min-insybc.replicas )

```
./kafka-topics.sh --create --bootstrap-server localhost:29092 --partitions 3 --replication-factor 1 --topic test-exception
```

## Creating a producer with wrong replica settings 

note: the default request-required-acks for kafka-console.producer.sh is acks=1, so if we have 1 replica for topic it works.

```
./kafka-console-producer.sh --broker-list localhost:29092 --request-required-acks=all --topic test-exception
```

# Exception ( NOT_ENOUGH_REPLICAS )

Situation : the producer should wait for all replicas, that means the number in min.insync.replicas.
however we have min.insync.replicas=2 for default settings in brokers, and just 1 replica for topic that causes a error.

```
>teste
>[2022-03-31 09:05:37,703] WARN [Producer clientId=console-producer] Got error produce response with correlation id 4 on topic-partition test-exception-1, retrying (2 attempts left). Error: NOT_ENOUGH_REPLICAS (org.apache.kafka.clients.producer.internals.Sender)
[2022-03-31 09:05:37,802] WARN [Producer clientId=console-producer] Got error produce response with correlation id 5 on topic-partition test-exception-1, retrying (1 attempts left). Error: NOT_ENOUGH_REPLICAS (org.apache.kafka.clients.producer.internals.Sender)
[2022-03-31 09:05:37,904] WARN [Producer clientId=console-producer] Got error produce response with correlation id 6 on topic-partition test-exception-1, retrying (0 attempts left). Error: NOT_ENOUGH_REPLICAS (org.apache.kafka.clients.producer.internals.Sender)
[2022-03-31 09:05:38,009] ERROR Error when sending message to topic test-exception with key: null, value: 5 bytes with error: (org.apache.kafka.clients.producer.internals.ErrorLoggingCallback)
org.apache.kafka.common.errors.NotEnoughReplicasException: Messages are rejected since there are fewer in-sync replicas than required.
```
