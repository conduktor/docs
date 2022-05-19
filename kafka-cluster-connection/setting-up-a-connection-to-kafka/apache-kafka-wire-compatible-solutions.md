---
description: >-
  While we don't officially support alternatives solutions to Apache Kafka, they
  follow the same protocol as Apache Kafka so they should work.
---

# Apache Kafka-wire compatible solutions

### Azure Event Hubs

Event Hubs is a fully managed, cloud-native service provided on Azure.

To configure Azure Event Hubs, follow their guide: [https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-for-kafka-ecosystem-overview](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-for-kafka-ecosystem-overview)

The concepts are similar:

| Kafka Concept  | Event Hubs Concept |
| -------------- | ------------------ |
| Cluster        | Namespace          |
| Topic          | Event Hub          |
| Partition      | Partition          |
| Consumer Group | Consumer Group     |
| Offset         | Offset             |

### Connecting to Event Hubs in Conduktor

You can use this configuration snippet:

```
bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

* Replace the password by following this guide [https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-get-connection-string](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-get-connection-string), this will give you something like:

```
... password="Endpoint=sb://mynamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXX";
```

### Redpanda

[Redpanda](https://redpanda.com/) is an Apache Kafka compatible event streaming platform. No Zookeeper, no JVM, and no code changes required.
