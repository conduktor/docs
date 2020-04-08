---
description: >-
  Overall, Conduktor cannot connect to Apache Kafka if using a Kafka CLI
  wouldn't work either.
---

# Impossible connection setups

## Conduktor is not \(yet\) magical

Overall, Conduktor cannot connect to Apache Kafka if using a Kafka CLI wouldn't work either.

If you know you cannot connect from your local computer to your Apache Kafka cluster using the Kafka CLI, this means that Conduktor will also not be able to connect. 

To test your connectivity to Kafka, you can run the following command:

```text
kafka-topics --list --bootstrap-server kafka-url:9092
```

If this commands work \(or a similar one to connect to your cluster\), then you should be able to use Conduktor. Otherwise, we're sorry but we cannot help. Please talk to your Kafka Administrator for more details. 

## Connecting to Kafka behind a bastion

This setup does not work:

&lt;TODO include screenshot &gt;

## Connecting to Kafka in a private network

This setup does not work

&lt; TODO include screenshot &gt;

If you're using a VPN, it would work

&lt; TODO include screenshot &gt;



