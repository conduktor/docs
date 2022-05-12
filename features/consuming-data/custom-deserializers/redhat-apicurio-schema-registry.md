# RedHat Apicurio Schema Registry

{% hint style="info" %}
This method is temporary as we plan to integrate with the Apicurio Serdes directly in Conduktor soon.
{% endhint %}

In order to successfully consume messages serialized with RedHat Apicurio Schema Registry, you first need to repackage the Apicurio deserializers into a single jar with all its dependencies. Here we'll use [Coursier](https://get-coursier.io/docs/cli-bootstrap#assemblies) but feel free to use maven plugins if you prefer.&#x20;

```
cs bootstrap io.apicurio:apicurio-registry-serdes-avro-serde:2.2.3.Final -M io.apicurio.registry.serde.avro.AvroKafkaDeserializer --assembly -o apicurio-registry-serdes-avro-serde-2.2.3.Final-with-dependancies.jar
```

We have also generated the fat jar for your convenience: [apicurio-registry-serdes-avro-serde-2.2.3.Final-with-dependancies.jar](https://github.com/conduktor/docs/raw/master/.gitbook/assets/apicurio-registry-serdes-avro-serde-2.2.3.Final-with-dependancies.jar)

Now import this jar file in your cluster configuration and use the class `AvroKafkaDeserializer` in the Custom Format (Plugin) from the Consume screen.

![](<../../../.gitbook/assets/Capture d’écran 2022-05-06 à 15.21.23.png>)

```
# Minimum required config with RedHat Registry
apicurio.registry.url=https://test.serviceregistry.rhcloud.com/t/d4d411af-xxxx-4184-xxxx-342e6cd03580/apis/registry/v2
apicurio.auth.username=srvc-acct-a95c41e8-xxxx-4e99-xxxx-217755ad7046
apicurio.auth.password=7d94b05a-xxxx-4f70-xxxx-1e6aba25a8b4
```

{% hint style="info" %}
You can further configure your deserializer as described in the Apicurio documentation: [https://www.apicur.io/registry/docs/apicurio-registry/2.2.x/getting-started/assembly-using-kafka-client-serdes.html#registry-serdes-config-consumer\_registry](https://www.apicur.io/registry/docs/apicurio-registry/2.2.x/getting-started/assembly-using-kafka-client-serdes.html#registry-serdes-config-consumer\_registry)
{% endhint %}
