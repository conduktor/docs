---
description: >-
  Overall, Conduktor cannot connect to Apache Kafka if using a Kafka CLI
  wouldn't work either.
---

# Impossible connection setups

## Conduktor is not \(yet\) magical

{% hint style="danger" %}
Overall, Conduktor cannot connect to Apache Kafka if using a Kafka CLI wouldn't work either.
{% endhint %}

If you know you cannot connect from your local computer to your Apache Kafka cluster using the Kafka CLI, this means that Conduktor will also not be able to connect. 

To test your connectivity to Kafka, you can run the following command:

```text
kafka-topics --list --bootstrap-server kafka-url:9092
```

If this commands work \(or a similar one to connect to your cluster\), then you should be able to use Conduktor. Otherwise, we're sorry but we cannot help. Please talk to your Kafka Administrator for more details. 

## Understanding Kafka Listeners

Part of understanding why your CLI \(and therefore Conduktor\) can't connect to your Kafka Cluster is due to your knowledge of how Kafka listeners work. As such, **we heavily recommend for you to read** [**this blog**](https://rmoff.net/2018/08/02/kafka-listeners-explained/) **to learn about Kafka listeners.**

## Connecting to Kafka behind a bastion / SSH hops

Connecting to Kafka through SSH with Conduktor is impossible, due to how the Kafka Clients work. No client can currently do this setup:

![](../../.gitbook/assets/image%20%288%29.png)

Because Conduktor or the Kafka CLI cannot leverage a tunneling connection to connect to your Kafka brokers

## Connecting to Kafka in a private cloud

This setup does not work:

![](../../.gitbook/assets/image%20%283%29.png)

Because even though your Kafka brokers are accessible through a public IP, upon connecting Conduktor \(and the Kafka Clients\) will be forced to use the private IP of Apache Kafka. This is a limitation of Apache Kafka due to [how the listeners work.](https://rmoff.net/2018/08/02/kafka-listeners-explained/)

If you're using a VPN, this would work:

![Using a VPN makes your local computer part of the private network](../../.gitbook/assets/image%20%2835%29.png)





