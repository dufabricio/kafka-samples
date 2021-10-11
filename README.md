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

## List Groups

```
./kafka-consumer-groups.sh --group grupoa --bootstrap-server localhost:29092 --describe
``# kafka-samples
