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

Consume from kafka topic and specify consumer group

```
kafka-console-consumer --topic <topic> --new-consumer --bootstrap-server <broker> --consumer-property group.id=<consumer_group_id>
```

#### kafka-avro-console-consumer

Consumer avro messages

```
kafka-avro-console-consumer --topic <topic> --new-consumer --bootstrap-server <broker> --from-beginning --property schema.registry.url=localhost:8081 --max-messages <num-messages>
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

List consumer groups

```
kafka-consumer-groups.sh --bootstrap-server <broker> -list
```

Describe consumer group / check consumer position / show lags

```
kafka-consumer-groups.sh --bootstrap-server <broker> --describe --group <group>
```

Delete consumer group

```
kafka-consumer-groups.sh --bootstrap-server <broker> --delete --group <group>
```

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

#### kafka-run-class

Get/List current offsets per partition for a topic

```
kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list <broker> --topic <topic>
```

#### kafka-configs

Set retention for a topic

```
kafka-configs --zookeeper <zookeeper> --alter --entity-type topics --entity-name <topic> --add-config retention.ms=3600000
```

Print all configuration overrides for a topic

```
kafka-configs --zookeeper <zookeeper> --describe --entity-type topics --entity-name <topic>
```

Delete a configuration override for retention.ms for a topic

```
kafka-configs --zookeeper <zookeeper> --alter --entity-type topics --entity-name <topic> --delete-config retention.ms 
```

#### kafka-streams-application-reset

Reset offset to latest of input topics and all the intermediary topics used by kafka streams

```
./kafka-streams-application-reset.sh --zookeeper <zookeeper> --application-id <consumer-group> --input-topics <topic1>... --intermediate-topics <intermediary-topic-1>... --to-latest
```

#### kafka-producer-perf-test

Perf test tool that produces messages to a topic:

```
kafka-producer-perf-test --topic <topic> --throughput 10000 --record-size 300 --num-records 20000 --producer-props bootstrap.servers="<broker>"
```

#### kafka-acls

Add a new consumer ACL for a topic

```
kafka-acls --authorizer-properties zookeeper.connect=<zookeeper> --add --allow-principal User:Bob --consumer --topic <topic> --group <group>
```

Add a new producer ACL to an existing topic

```
kafka-acls --authorizer-properties zookeeper.connect=<zookeeper> --add --allow-principal User:Bob --producer --topic <topic>
```

List ACLs for a topic

```
kafka-acls --authorizer-properties zookeeper.connect=<zookeeper> --list --topic <topic>
```

#### consumer offsets

List consumer offsets

```
kafka-console-consumer --topic __consumer_offsets --bootstrap-server=<broker> --formatter "kafka.coordinator.group.GroupMetadataManager\$OffsetsMessageFormatter"
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

Copy messages from a source topic to another topic

```
kafkacat -b localhost:9092 -C -t source-topic -K: -e -o beginning | \
kafkacat -b localhost:9092 -P -t target-topic -K: 
```


--

#### docker

Run kafka tools using confluent docker image

```
docker run -it --rm confluent/tools kafka-topics --list --bootstrap-server <broker>
```

---

Run kafka command in kubernetes using confluent docker image

```
kubectl --namespace=<namespace> run --generator=run-pod/v1 cmd-kafka --image confluent/tools --rm -ti --command -- kafka-topics --list --bootstrap-server <broker>
```

