## kafka-cheatsheet
### Kafka cheatsheet


#### kafka-console-consumer

Consume from kafka topic

```
kafka-console-consumer.sh --bootstrap-server <broker> --topic <topic> --property print.timestamp=true --property print.key=true
```

Properties: 

- Print message key: `--property print.key=true`

- Print timestamp: `--property print.timestamp=true`


Consume from kafka topic a max number of messages

```
kafka-console-consumer.sh --bootstrap-server <broker> --topic <topic> --max-messages <max-messages>
```

Consume from kafka topic/partition & offset

```
kafka-console-consumer.sh --bootstrap-server <broker> --topic <topic> --offset <offset> --partition <partition-num>
```


#### kafka-console-producer

Produce to kafka topic

```
kafka-console-producer.sh --broker-list <broker> --topic <topic>
```

Produce messages from a file

```
kafka-console-producer.sh --broker-list <broker> --topic <topic>
--new-producer < my_file.txt
```

#### kafka-topics

Show / display kafka version

```
kafka-topics.sh --bootstrap-server <broker> --version
```

Create topic

```
kafka-topics.sh --create --bootstrap-server <broker> --replication-factor <replication-factor> --partitions <partitions> --topic <new-topic>
```

Describe topic

```
kafka-topics.sh --bootstrap-server <broker> --describe --topic <topic>
```

Delete topic

```
kafka-topics.sh --create --bootstrap-server <broker> --topic <topic>
```

List topics

```
kafka-topics.sh --list --bootstrap-server <broker>
```

List / show partitions whose leader is not available

```
kafka-topics.sh --bootstrap-server <broker> --describe --under-replicated-partitions
```


List / show partitions whose isr-count is less than the configured minimum

```
kafka-topics.sh --bootstrap-server <broker> --describe --under-min-isr-partitions
```

List / show under replicated partitions

```
kafka-topics.sh --bootstrap-server <broker> --describe --under-replicated-partitions
```

List with overriden configs / list intermediary / stores / changelogs topics

```
kafka-topics.sh --bootstrap-server <broker> --describe --topics-with-overrides
```

Alter / modify / update number of partitions for a topic

```
kafka-topics.sh --bootstrap-server <broker> --topic <topic> --alter --partitions <partitions>
```

Alter / modify / update replication-factor for a topic

```
kafka-topics.sh --bootstrap-server <broker> --topic <topic> --alter --replication-factor <replication-factor>
```

#### kafka-consumer-groups

Reset offsets for a topic to latest offset for a consumer group

```
kafka-consumer-groups.sh --bootstrap-server <broker> --group <group> --execute --reset-offsets --to-latest --topic <topic> —execute
```

Reset offsets for a topic to earliest offset for a consumer group

```
kafka-consumer-groups.sh --bootstrap-server <broker> --group <group> --execute --reset-offsets --to-earliest --topic <topic> —execute
```

Reset offsets for a topic to specific offset for a consumer group

```
kafka-consumer-groups.sh --bootstrap-server <broker> --group <group> --execute --reset-offsets --to-offset <offset> --topic <topic> —execute
```

Reset offsets for a topic to a datetime (with format 'YYYY-MM-DDTHH:mm:SS.sss') for a consumer group

```
kafka-consumer-groups.sh --bootstrap-server <broker> --group <group> --execute --reset-offsets --to-datetime <datetime> --topic <topic> —execute
```

Delete consumer group

```
kafka-consumer-groups.sh --bootstrap-server <broker> --delete --group <group>
```

Describe consumer group / check consumer position / show lags

```
kafka-consumer-groups.sh --bootstrap-server <broker> --describe --group <group>
```

List consumer groups

```
kafka-consumer-groups.sh --bootstrap-server <broker> -list
```

#### kafka-run-class

Get/List current offsets per partition for a topic

```
kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list <broker> --topic <topic>
```

#### kafka-streams-application-reset

Reset offset to latest of input topics and all the intermediary topics used by kafka streams

```
./kafka-streams-application-reset.sh --zookeeper <zookeeper> --application-id <consumer-group> --input-topics <topic1>... --intermediate-topics <intermediary-topic-1>... --to-latest
```

--

#### kafkacat

Install kafkacat in mac using brew

```
brew install kafkacat
```

Install kafkacat in debian systems

```
apt-get install kafkacat
```

Status of kafka broker / metadata listing

```
kafkacat -b <broker> -L
```

Metadata listing in json format

```
kafkacat -b <broker> -L -J
```

Produce message

```
kafkacat -P -b <broker> -t <topic>
```

Produce message with header

```
kafkacat -P -b <broker> -t <topic>
```

Produce message with snappy compression

```
kafkacat -P -b <broker> -t <topic> -z snappy
```

Read / consume messages

```
kafkacat -b <broker> -t <topic> -C -f '\nKey (%K bytes): %k\t\nValue (%S bytes): %s\nTimestamp: %T\tPartition: %p\tOffset: %o\n--\n'
```

Read X number of messages from a topic

```
kafkacat -C -b <broker> -t <topic> -p <partition> -o -<num_messages> -e
```

Consume from all partitions of a topic

```
kafkacat -C -b <broker> -t <topic>
```

Consume messages and wrap them in a JSON envelope

```
kafkacat -b <broker> -t <topic> -J
```

Consume avro message with key and value encoded in avro

```
kafkacat -b <broker> -t <topic> -s avro -r <schema-registry>
```

Consume avro message with avro value

```
kafkacat -b <broker> -t <topic> -s value=avro -r <schema-registry>
```

Consume avro message with key encoded in avro

```
kafkacat -b <broker> -t <topic> -s key=avro -r <schema-registry>
```

Produce a tombstone (a "delete" for compacted topics) for a key by providing an empty (NULL) message value

```
echo "<key>:" | kafkacat -b <broker> -t <topic> -Z -K: 
```

Produce message with header

```
kafkacat -P -b <broker> -t <topic> -H "<header_key>:<header_value>"
```

Consume messages and its headers

```
kafkacat -b <broker> -C -t <topic> -f 'Headers: %h: Message value: %s\n'
```

Enable the idempotent producer with exactly-once and strict-ordering guarantees

```
kafkacat -b <broker> -X enable.idempotence=true -P -t <topic>
```

Query offset by Timestamp

```
kafkacat -b <broker> -Q -t <topic>:<partition>:<epoch_timestamp>
```

Consume messages between two timestamps

```
kafkacat -b <broker> -C -t <topic> -o s@<epoch_start_timestamp> -o e@<epoch-end_timestamp>
```

---

Run kafka tools using confluent docker image

```
docker run -it --rm confluent/tools kafka-topics --list --bootstrap-server <broker>
```

---

Run kafka command in kubernetes using confluent docker image

```
kubectl --namespace=<namespace> run --generator=run-pod/v1 cmd-kafka --image confluent/tools --rm -ti --command -- kafka-topics --list --bootstrap-server <broker>
```
