---
description: >-
  Connecting to a secure Kafka cluster may be difficult, but hopefully this page
  will help you!
---

# Connecting to a Secure Kafka

## General Idea

Conduktor leverages the default Apache Kafka Java Clients, and therefore we use the same [configuration properties](https://kafka.apache.org/documentation/#consumerconfigs). If you are trying to connect to a secure Kafka cluster using Conduktor, please first try to use the CLI to manage to do it. If you don't know how, please contact your administrator. 

**Example:**

```bash
kafka-console-consumer \
    --topic my-topic \
    --bootstrap-server SASL_SSL://kafka-url:9093 \
    --consumer.config config.properties
```

Your `config.properties` file may contain something like this:

```text
security.protocol=SSL
ssl.truststore.location=/var/private/ssl/client.truststore.jks
ssl.truststore.password=test1234
other.configs=values...
```

What Conduktor needs to connect to a secure Kafka cluster is all the values from your `config.properties` file.

{% hint style="info" %}
In case you don't know what should be the values in the `config.properties` file, please contact your Kafka administrator. 

**Note:** these are the same properties you would use in your Kafka Java clients or applications. 
{% endhint %}

## SSL Configuration

Show example configuration

{% hint style="warning" %}
Please make sure to put the **full paths** to your SSL certificates in your properties file  
Relative paths may not work for Conduktor. 
{% endhint %}

## SASL Configuration

Show example configuration

{% hint style="warning" %}
Please make sure to put the **full paths** to your SASL key files in your properties file  
Relative paths may not work for Conduktor. 
{% endhint %}

## Can you help us?

Unfortunately, we cannot provide support to help you connect to your secure cluster besides what's included in the documentation. **Your Kafka administrator will have the answer to your problem**, please send them the link to this documentation page. Thank you!

