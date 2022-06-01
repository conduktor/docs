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

We support the most common formats, but also bytes, JSON, and multiple Avro and Protobuf flavours. We are compatible with the **Confluent Schema Registry** and the **AWS Glue Registry** but we also support Avro and Protobuf data **not** written using the Confluent Schema Registry. You can even provide your own Avro or Protobuf schema directly within Conduktor (see [Schema without Confluent Schema Registry](pick-your-format-wisely.md#without-confluent-schema-registry)).

## Key and Value

There are 2 formats to pick: the key format and the value format.

* In most cases, the key is a String so nothing to worry about here. But Conduktor also support various formats

![List of supported key formats](../../.gitbook/assets/key\_formats.png)

* The value is typically JSON or Avro or Protobuf but it can also be long, float or other primitives when working with Kafka Streams where you're doing aggregations for instance

![List of supported value formats](../../.gitbook/assets/value\_formats.png)

{% hint style="info" %}
Conduktor automatically pick "Avro (Auto)" when it detects a matching Confluent Schema Registry subject (if configured)
{% endhint %}

{% hint style="warning" %}
If you don't see "Avro (Auto)", it means you didn't configured the url of your schema registry in your cluster configuration: [check the doc](https://docs.conduktor.io/kafka-cluster-connection/setting-up-a-connection-to-kafka#schema-registry).
{% endhint %}

## Without Confluent Schema Registry ?

Most Kafka installations rely on the Confluent Schema Registry, that's the JSON`(Auto)` , `Avro (Auto)` and `Protobuf (Auto)` formats.&#x20;

When data flow in, Conduktor knows the Avro schema to use because it's in the payload itself (well, the id only, but that's enough) if we follow the Confluent's convention.

But not everyone follows this convention! This means it's possible that the record does not contain the schema to read the data, and you need to help Conduktor.

### With Avro, you can help Conduktor in 3 ways:

* Which subject to use : `Avro (Subject)` (if using Confluent Schema Registry)
* Which schemaId to use : `Avro (Schema ID)` (if using Confluent Schema Registry)
* Which schema to use : `Avro (Custom)` (no need to the registry)

![Avro custom](../../.gitbook/assets/value\_formats\_avro.png)

For instance, I want to provide an Avro Schema to read my data:

* I choose `Avro (Custom)` format
* I paste my custom schema into the textarea
* I check "Confluent Encoding" because in my case, data are encoded using Confluent's convention
  * In most cases, this will NOT be checked if we're using this feature
* The data will be read using this schema!

{% hint style="success" %}
It can be used to "project" the data: read and display only a subset of fields
{% endhint %}

```javascript
{
  "type" : "record",
  "name" : "SearchRequest",
  "namespace" : "com.mycompany",
  "fields" : [ {
    "name" : "query",
    "type" : "string"
  }, {
    "name" : "page_number",
    "type" : "int"
  }, {
    "name" : "result_per_page",
    "type" : "int"
  } ]
}
```

![Consume using custom Avro schema](<../../.gitbook/assets/image (52) (1).png>)

### With Protobuf, provide the schema

Like `Avro (Custom)` format, the`Protobuf (Custom)` allows you to paste a raw Protobuf 3 schema to decode messages.

The workflow is the same as Avro :&#x20;

* &#x20;I choose `Protobuf (Custom)` format
* I paste my custom proto3 schema into the textarea
* The data will be read using this schema!

```protobuf
syntax = "proto3";

message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}
```

![Consume using custom Protobuf Schema](<../../.gitbook/assets/image (54) (1).png>)

{% hint style="info" %}
Conduktor will iterate over all root message types in the provided schema and select the best one for each message (the first type decoding without missing or unknown fields).
{% endhint %}

### Using AWS Glue Registry
With AWS Glue Registry, the registry name and the schema information (including the format) are embedded in the message. 
All you need to do is provide the AWS region of your registry.

![Consume with AWS Glue Registry](<../../.gitbook/assets/aws-glue-consume-config.png>)

You also need to have your AWS credentials configured in your system 
(see [AWS documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)).

Conduktor will deserialize your messages (Json, Avro, Protobuf format are supported) and display them in a json format.
## Possible errors (magic byte)

If you try to consume Avro data without properly configuring it (and because it was not auto-detected by Conduktor for some reasons), you'll end up with garbage data like this:

![](<../../.gitbook/assets/screenshot-2020-06-25-at-16.13.10 (1).png>)

On the contrary, if you try to read non-Avro data using the Avro format, you'll end up with a list of errors (Conduktor doesn't stop, in case it was test records for instance):

{% hint style="danger" %}
ERR: Unknown magic byte!
{% endhint %}

![](<../../.gitbook/assets/screenshot-2020-06-25-at-16.15.21 (1).png>)

## Custom deserializers

See the dedicated documentation page: [Custom deserializers](custom-deserializers/)
