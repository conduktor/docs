# Schema Registry

{% hint style="info" %}
Work In Progress
{% endhint %}

## Supported Formats

Conduktor handles Avro, Protobuf, and JSON Schemas.



## Managing soft and hard deletion of Subjects and Schemas



## How to Connect to a secured Schema Registry?

Add this in the additional properties of the Schema Registry \(notice the `schema.registry.` prefix\).

```text
schema.registry.ssl.truststore.location=/kafkassl/kafka.client.truststore.jks
schema.registry.ssl.trusstore.password=<xxx>
schema.registry.ssl.keystore.location=/kafkassl/kafka.client.keystore.jks
schema.registry.ssl.keystore.password=<xxx>
schema.registry.ssl.key.password=<xxx>
```

## Using Aiven's Schema Registry \(Karapace\)?

If you are running Kafka at Aiven's \(good idea!\), then you're using karapace as the Schema Registry. It's totally different of the Confluent Schema Registry. It's almost 100% compatible.

{% embed url="https://github.com/aiven/karapace/" %}







