---
description: >-
  Overall, Conduktor cannot connect to Apache Kafka if using the classic Apache
  Kafka CLI wouldn't work either.
---

# Understanding Connectivity Issues

## Conduktor is not (yet) magical: Apache Kafka limitations

If you know you cannot connect from your local computer to your Apache Kafka cluster using the Kafka CLI, this means that Conduktor will also not be able to connect. To test your connectivity to Kafka, you can run the following command:

```
kafka-topics --list --bootstrap-server kafka-url:9092
```

If this command work (or a similar one to connect to your cluster), then you should be able to use Conduktor. Otherwise, we're sorry but we cannot help. Please talk to your Apache Kafka Administrator for more details.&#x20;

This will typically be a one of these problems:

* your Apache Kafka `advertised.listeners` are configured using internal IPs/DNS that your computer cannot resolve
* you're using Docker and you did not expose the Kafka ports to your host, where Conduktor is running
* your computer cannot resolve the IPs/DNS exposed by Apache Kafka because your cluster is running in a private subnet not known to your computer

{% hint style="success" %}
Just remember that Conduktor is running on **your** machine, running on **your** network, which may be _different_ from the network your Apache Kafka clusters are running.
{% endhint %}

## Understanding Kafka Listeners

Part of understanding why your CLI (and therefore Conduktor) can't connect to your Kafka Cluster is due to your knowledge of how the Apache Kafka listeners work. As such, we **heavily** recommend for you to read [>> this blog <<](https://rmoff.net/2018/08/02/kafka-listeners-explained/) to learn about Kafka listeners.

> Another longer blog can be found at: [https://www.confluent.io/blog/kafka-client-cannot-connect-to-broker-on-aws-on-docker-etc/](https://www.confluent.io/blog/kafka-client-cannot-connect-to-broker-on-aws-on-docker-etc/)

![internal + external listeners configured](<../../.gitbook/assets/image (48).png>)

## Connecting to Kafka behind a bastion / SSH hops

Connecting to Apache Kafka through a simple SSH tunnel with Conduktor is impossible, due to how the Apache Kafka protocol works. No client can currently do this setup:

![](<../../.gitbook/assets/image (8).png>)

You may try by running a special kafka-proxy as mentioned in [Connect to Amazon MSK](connect-to-amazon-msk.md), where it's a common use-case.

## Connecting to Kafka in a private cloud

This setup does not work, because your computer will not be able to resolve the private subnet IP/DNS exposed by Apache Kafka:

![](<../../.gitbook/assets/image (3) (1).png>)

Because even though your Kafka brokers are accessible through a public IP, upon connecting Conduktor (and the Kafka Clients) will be forced to use the private IP of Apache Kafka. This is a limitation of Apache Kafka due to [how the listeners work.](https://rmoff.net/2018/08/02/kafka-listeners-explained/) Please read the section above _Understanding Kafka Listeners_.

If you're using a VPN, this would work because your computer will be able to resolve the private subnet IP/DNS:

![Using a VPN makes your local computer part of the private network](<../../.gitbook/assets/image (35).png>)



