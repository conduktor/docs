# AWS Glue Schema Registry

{% hint style="info" %}
This method is temporary as we plan to integrate with the Apicurio Serdes directly in Conduktor soon.
{% endhint %}

In order to successfully consume messages serialized with AWS Glue Registry, you first need to repackage the Glue deserializers into a single jar with all its dependancies. Here we'll use [Coursier](https://get-coursier.io/docs/cli-bootstrap#assemblies) but feel free to use maven plugins if you prefer.&#x20;

```
# Example with MacOS
brew install coursier/formulas/coursier
cs setup
cs bootstrap software.amazon.glue:schema-registry-serde:1.1.10 --assembly -M com.amazonaws.services.schemaregistry.deserializers.GlueSchemaRegistryKafkaDeserializer -o glue-schema-registry-serde-1.1.10-with-dependancies.jar
```

We have also generated the fat jar for your convenience: [glue-schema-registry-serde-1.1.10-with-dependancies.jar](https://github.com/conduktor/docs/raw/master/.gitbook/assets/glue-schema-registry-serde-1.1.10-with-dependancies.jar)

Now import this jar file in your cluster configuration and use the class `GlueSchemaRegistryKafkaDeserializer` in the Custom Format (Plugin) from the Consume screen.

{% hint style="success" %}
Configure your deserializer using the Glue documentation : &#x20;

* [https://docs.aws.amazon.com/glue/latest/dg/schema-registry-gs.html#schema-registry-gs-serde](https://docs.aws.amazon.com/glue/latest/dg/schema-registry-gs.html#schema-registry-gs-serde)
* [https://github.com/awslabs/aws-glue-schema-registry](https://github.com/awslabs/aws-glue-schema-registry)
{% endhint %}

```
# Minimum required config with AWS Glue Registry
region=us-west-2
```
