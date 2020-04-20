# Connecting to Kafka running under Docker

## General Case

Connecting to Kafka under Docker is the same as connecting to a normal Kafka cluster. If your cluster is accessible from the network, and the advertised hosts are setup correctly, we will be able to connect to your cluster.

Read more about Kafka Listeners here: 

{% embed url="https://rmoff.net/2018/08/02/kafka-listeners-explained" %}



## Kafka on Docker on Localhost

To run Kafka on Docker on Localhost properly, we recommend you use this project:

{% embed url="https://github.com/simplesteph/kafka-stack-docker-compose" %}

The README should tell you how to get started.

Then, you can connect to Kafka on localhost using Conduktor!



