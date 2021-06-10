---
description: >-
  Conduktor supports many formats and growing. From the basic formats to the
  most useful, JSON, Apache Avro, Protobuf, with or without Schema Registry.
---

# Pick your format wisely



{% hint style="info" %}
This is the first thing to setup when you're consuming a topic
{% endhint %}

Conduktor supports many formats when deserializing data and we keep adding some.

We don't support Protobuf yet, but it's on our roadmap. If you have this need or any other specific needs, don't hesitate to [contact us](https://www.conduktor.io/contact/).

We support the most common formats, but also bytes, JSON, and multiple Avro flavours. We are compatible with the **Confluent Schema Registry** but we also support Avro data **not** written using the Confluent Schema Registry. You can even provide your own Avro schema directly within Conduktor.

## Key and Value

There are 2 format to pick: the key format and the value format.

* In most cases, the key is a string so nothing to worry about here.
* the value is typically JSON or Avro
  * it can be long, float or other primitives when working with Kafka Streams where you're doing aggregations for instance

{% hint style="info" %}
Conduktor automatically pick Avro when it detects a matching Confluent Schema Registry subject \(if configured\)
{% endhint %}

![](../../.gitbook/assets/screenshot-2020-06-25-at-16.06.03.png)

{% hint style="warning" %}
If you don't see "Avro \(Schema Registry\)", it means you didn't configured the url of your schema registry in your cluster configuration: [check the doc](https://docs.conduktor.io/kafka-cluster-connection/setting-up-a-connection-to-kafka#schema-registry).
{% endhint %}

## Avro without Confluent ?

Most Kafka installations rely on the Confluent Schema Registry, that's the Avro \(Schema Registry\) format. When data flow in, Conduktor knows the Avro schema to use because it's in the payload itself \(well, the id only, but that's enough\) if we follow the Confluent's convention.

But not everyone follows this convention! This means it's possible that the record does not contain the schema to read the data, and you need to help Conduktor. 

You can help Conduktor in 3 ways:

* Which subject to use \(if using Confluent Schema Registry\)
* Which schemaId to use \(if using Confluent Schema Registry\)
* Which schema to use \(no need to the registry\)

![The special Avro format at the bottom of the list](../../.gitbook/assets/screenshot-2020-06-25-at-16.31.52.png)

For instance, I want to provide a custom Avro Schema to read my data:

* I choose "Avro \(Custom Schema"\)
* I paste my custom schema
* I check "Confluent Encoding" because in my case, data are encoded using Confluent's convention
  * In most cases, this will NOT be checked if we're using this feature
* The data will be read using this schema!

{% hint style="success" %}
It can be used to "project" the data: read and display only a subset of fields
{% endhint %}

```javascript
{
  "type": "record",
  "name": "myrecord",
  "namespace": "com.company.events",
  "fields": [
    {
      "name": "id",
      "type": "string"
    },
    {
      "name": "type",
      "type": "string",
      "default": "some default"
    }
  ]
}

```

![](../../.gitbook/assets/screenshot-2020-06-25-at-16.34.44%20%282%29.png)



## Possible errors \(magic byte\)

If you try to consume Avro data without properly configuring it \(and because it was not auto-detected by Conduktor for some reasons\), you'll end up with garbage data like this:

![](../../.gitbook/assets/screenshot-2020-06-25-at-16.13.10%20%281%29.png)

On the contrary, if you try to read non-Avro data using the Avro format, you'll end up with a list of errors \(Conduktor doesn't stop, in case it was test records for instance\):

{% hint style="danger" %}
ERR: Unknown magic byte!
{% endhint %}

![](../../.gitbook/assets/screenshot-2020-06-25-at-16.15.21%20%281%29.png)



