---
description: >-
  It's an important aspect when using Kafka and its extensions (Kafka Streams,
  Kafka Connect, ksqlDB..) to know what's going on behind the scene!
---

# Monitoring

Currently, Conduktor provides only a few features from the metrics system. We only handle the metrics when the Kafka brokers expose their metrics using JMX or Jolokia \(JMX over HTTP\). 

## Configuration

In your Cluster Configuration, you can enable either JMX or Jolokia access for Conduktor to access the metrics.

### JMX

JMX is a common protocol used in Java applications to expose metrics. Apache Kafka is a Java application, and exposes naturally tons of JMX metrics.

{% hint style="warning" %}
JMX is not an HTTP protocol, it cannot be accessed through an Internet browser
{% endhint %}

![](../.gitbook/assets/2021-02-03-14-04-03-metrics.png)

With this configuration, Conduktor will regularly fetch the JMX endpoints:

* &lt;broker advertised listeners&gt;:9999, using the JMX protocol

The &lt;broker advertised listeners&gt; are **NOT** the bootstrap address you have configured in Conduktor to connect to your Kafka cluster. These things are entirely different, this is a specificity of how Kafka works. Please refer to [understanding-kafka-listeners](https://docs.conduktor.io/kafka-cluster-connection/setting-up-a-connection-to-kafka/impossible-connection-setups#understanding-kafka-listeners) if you have no idea what we are talking about.

Therefore, your computer must be able to connect to this address:port for Conduktor to fetch the metrics.

Conduktor also supports connecting to JMX endpoints and registries through both one and two-way SSL. 

![](../.gitbook/assets/2021-02-03-14-47-56-metrics-ssl.png)

To enable JMX over SSL, specify the following JVM arguments when starting the brokers : 

```text
-Dcom.sun.management.jmxremote.registry.ssl=true \
-Djavax.net.ssl.keyStore=/path/to/keystore.jks \
-Djavax.net.ssl.keyStorePassword=keystore_password \
-Djavax.net.ssl.trustStore=/path/to/truststore.jks \
-Djavax.net.ssl.trustStorePassword=truststore_password 
```

and add in Conduktor the client truststore containing the server key certificate.

To enable mutual TLS \(recommended\), add the following JVM argument :

```text
-Dcom.sun.management.jmxremote.ssl.need.client.auth=true
```

and add in Conduktor the client keystore, containing the certificate that must be trusted by the server.

To enable the secured RMI registry, make sure the brokers are started with the following JVM argument :

```text
-Dcom.sun.management.jmxremote.registry.ssl=true
```

and check the "Secured RMI Registry" checkbox in Conduktor

### Jolokia

Jolokia is a technology to expose a HTTP server to access JMX metrics over a simple HTTP request.

It's often started as a Java agent directly on the main Java program. When Jolokia starts, it starts an HTTP server on a configurable port \(default: 8778\).

{% hint style="info" %}
This is the power of Jolokia. Because it exposes the metrics through, they are accessible through an Internet browser
{% endhint %}

![](../.gitbook/assets/screenshot-2021-01-17-at-21.56.35.png)

With this configuration, Conduktor will regularly fetch the HTTP\(S\) endpoints:

* &lt;broker advertised listeners&gt;:8778/jolokia

The &lt;broker advertised listeners&gt; are **NOT** the bootstrap address you have configured in Conduktor to connect to your Kafka cluster. These things are entirely different, this is a specificity of how Kafka works. Please refer to [understanding-kafka-listeners](https://docs.conduktor.io/kafka-cluster-connection/setting-up-a-connection-to-kafka/impossible-connection-setups#understanding-kafka-listeners) if you have no idea what we are talking about.

Therefore, your computer must be able to connect to this address:port for Conduktor to fetch the metrics.

## Usage in Conduktor

For now, Conduktor barely used the power of the metrics. We're working to expose common metrics to monitor as graphs and notify you if sometimes looks wrong.

* **Version**: without metrics, Conduktor can only apply some heuristics to determine the version of the brokers. With metrics, we can know exactly which one it is!
  * It's important to be sure that all your version are up-to-date and similar

![](../.gitbook/assets/screenshot-2021-01-18-at-00.08.51.png)

* **Topic throughput**: we display the throughput \(in/out\). Useful to know the activity of a topic. Does it have many consumers, or many producers, or none?

![](../.gitbook/assets/screenshot-2021-01-18-at-00.13.05.png)

* **Rolling Restart**
  * This is an ops oriented feature, only available when you host your own Kafka and when Conduktor can ssh into the machines.
  * The metrics are necessary to monitor the Under Replicated Partitions

![](../.gitbook/assets/screenshot-2021-01-18-at-00.15.37.png)

## More to come

* Add Prometheus support
* A Monitoring dashboard with graphs to monitor well-known metrics
* Alert you within Conduktor in case of metrics issue detection
* Integrate the topic metrics into other screens



