# AWS Glue Schema Registry

{% hint style="info" %}
This method is temporary as we plan to integrate with the AWS Glue Registry Serdes directly in Conduktor soon.
{% endhint %}

In order to successfully consume messages serialized with AWS Glue Registry, you first need to repackage the Glue deserializers into a single jar with all its dependencies. Here we'll use [Coursier](https://get-coursier.io/docs/cli-bootstrap#assemblies) but feel free to use maven plugins if you prefer.&#x20;

```
cs bootstrap software.amazon.glue:schema-registry-serde:1.1.10 --assembly -M com.amazonaws.services.schemaregistry.deserializers.GlueSchemaRegistryKafkaDeserializer -o glue-schema-registry-serde-1.1.10-with-dependancies.jar
```

We have also generated the fat jar for your convenience: [glue-schema-registry-serde-1.1.10-with-dependancies.jar](https://github.com/conduktor/docs/raw/master/.gitbook/assets/glue-schema-registry-serde-1.1.10-with-dependancies.jar)

Now import this jar file in your cluster configuration and use the class `GlueSchemaRegistryKafkaDeserializer` in the Custom Format (Plugin) from the Consume screen.

![](<../../../.gitbook/assets/Capture d’écran 2022-05-06 à 15.18.19.png>)

```
region=us-west-2
avroRecordType=GENERIC_RECORD
```

{% hint style="success" %}
You can further configure your deserializer as described in the Glue documentation: &#x20;

* [https://docs.aws.amazon.com/glue/latest/dg/schema-registry-gs.html#schema-registry-gs-serde](https://docs.aws.amazon.com/glue/latest/dg/schema-registry-gs.html#schema-registry-gs-serde)
* [https://github.com/awslabs/aws-glue-schema-registry](https://github.com/awslabs/aws-glue-schema-registry)
{% endhint %}

{% hint style="warning" %}
The Glue library doesn't pick the AWS profile from the Kafka config `sasl.jaas.config`. \
If you need to use a different profile than the `default`, you must set the following environment variable:\
`AWS_PROFILE="<your-profile>"`
{% endhint %}
