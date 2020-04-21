# Connecting to Kafka running under Docker

## General Case

Connecting to Kafka under Docker is the same as connecting to a normal Kafka cluster. If your cluster is accessible from the network, and the advertised hosts are setup correctly, we will be able to connect to your cluster.

Read more about Kafka Listeners here: 

{% embed url="https://rmoff.net/2018/08/02/kafka-listeners-explained" %}



## Kafka on Docker on Localhost

### Method 1 \(recommended\)

To run Kafka on Docker on Localhost properly, we recommend you use this project:

{% embed url="https://github.com/simplesteph/kafka-stack-docker-compose" %}

The README should tell you how to get started.

Example

```text
docker-compose -f zk-single-kafka-single.yml up
```

Then, you can connect to Kafka on `127.0.0.1:9092` using Conduktor!

### Method 2

We can leverage this project from lenses.io 

{% embed url="https://github.com/lensesio/fast-data-dev" %}

Please read the README file to see how to launch this properly. 

**For example, on Mac OS X or Windows 10 running Docker**

```text
docker run -p 2181:2181 -p 3030:3030 -p 8081-8083:8081-8083  -p 9581-9585:9581-9585 -p 9092:9092 -e ADV_HOST=127.0.0.1  -e RUNNING_SAMPLEDATA=1 lensesio/fast-data-dev
```

Conduktor can then connect to `127.0.0.1:9092` 

**If on Docker Toolbox:**

```text
docker run -p 2181:2181 -p 3030:3030 -p 8081-8083:8081-8083  -p 9581-9585:9581-9585 -p 9092:9092 -e ADV_HOST=192.168.99.100  -e RUNNING_SAMPLEDATA=1 lensesio/fast-data-dev
```

Conduktor can then connect to `192.189.99.100:9092` 

