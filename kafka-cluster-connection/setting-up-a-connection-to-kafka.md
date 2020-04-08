---
description: >-
  Conduktor allows you to easily add, manage and save cluster connection
  configuration.
---

# Setting up a connection to Kafka

## Adding a Cluster

To add a cluster, please click on the "Add new cluster" button

![](../.gitbook/assets/image%20%2815%29.png)

This will open up a list of configurations you can set for Conduktor

![](../.gitbook/assets/image%20%2825%29.png)

### Kafka Cluster

this tab is the most important tab, and you have the following options:

* **integration:** in case you are using Aiven or Confluent Cloud, you have the possibility to quickly integrate with them through a dialog. In case you're not using those, rest assured you can connect to your cluster!
* **cluster name:** this will be the name of your cluster when navigating the list of clusters on the right-hand side of the Conduktor splash screen
* **bootstrap servers:** list of Kafka brokers to connect to, usually providing two or three brokers is enough. In the form of `broker1-url:9092,broker2-url:9092`, or `PLAINTEXT://broker1-url:9092,broker2-url:9092`
* **zookeeper \(optional\):** providing a Zookeeper URL will allow you to access more features through Conduktor, for example partition reassignment. Over time, as Kafka migrates away from using Zookeeper, this configuration will become less and less necessary to provide. In the form of `zookeeper1-url:2181,zookeeper2-url:2181`
* **additional properties:** this is one of the most important section, especially if you have a [secure Kafka cluster](connecting-to-a-secure-kafka.md). Any additional properties you usually provide to your CLI or your Java clients should be inserted here. For more information, please read the [secure Kafka cluster section](connecting-to-a-secure-kafka.md).

You can use the two buttons to test the Kafka and Zookeeper connectivity to ensure you connection details are accurate. 

### Schema Registry

In case you are using the [Confluent Schema Registry](https://docs.confluent.io/current/schema-registry/index.html) and usually Avro data, you should use this tab to setup the connection details to your registry. These detail are necessary to activate the "Schema Registry" tab in Conduktor, as well as consume and produce data in Avro format. 

![](../.gitbook/assets/image%20%2813%29.png)

* **URL:** HTTP or HTTPS endpoint of your schema registry. 
* **Security:** Choose the security type \(None, Basic Auth, Bearer Token\)
* **Additional Properties:** any key value pair needed to make the connection work

After entering the necessary details, you can test the connectivity to your schema registry

{% hint style="warning" %}
In case of issues connecting to an HTTPS Schema Registry, or if you're using a specific security plugin, [please contact us](https://www.conduktor.io/contact). We are interested in making this process easier for you. 
{% endhint %}

### Kafka Connect

Here you can add a list of Kafka Connect clusters that are linked to this Kafka cluster. 

![](../.gitbook/assets/image%20%2820%29.png)

Each Kafka Connect cluster will have

* **Name:** unique name to identify a connect cluster
* **URL:** HTTP or HTTPS endpoint of your connect cluster
* **Security:** basic authentication \(if secure connect cluster\), as well as key and trust store locations in case of TLS encryption. 

You can also test the connectivity to your Connect clusters from there. 

### Metrics

Enabling metrics allows Conduktor to get real-time features, statistics, monitoring over your cluster, as well as the rolling restart feature. 

![](../.gitbook/assets/image%20%2826%29.png)

We current support Jolokia and JMX to extract metrics. 

* **Jolokia:** please provide the protocol as well as the Jolokia port. The [Jolokia agent](https://jolokia.org/agent.html) should be installed on all your Kafka brokers. 
* **JMX:** if you have started the JMX utility on your Kafka brokers, please provide the JMX port for Conduktor to connect to. 

{% hint style="info" %}
Please contact us in case you need other metrics mechanisms. 
{% endhint %}

### SSH

{% hint style="warning" %}
The SSH configuration is quite powerful and should only be allowed to be set-up for your Kafka administrators. Average Kafka users should not need to fill up the details of that tab. 
{% endhint %}

The SSH configuration enables Conduktor to directly access your brokers machines, enabling features like the rolling restart feature. 

![](../.gitbook/assets/image%20%2822%29.png)

Please provide the port, user, authentication method \(password or SSH key pair\) and test the SSH configuration. 

## Testing the Connection to a Cluster

Conduktor has utilities to test the connectivity to your Kafka clusters. In case you cannot establish a connectivity to your clusters, please make sure to read the following two pages:

{% page-ref page="connecting-to-a-secure-kafka.md" %}

{% page-ref page="impossible-connection-setups.md" %}

## Multi Cluster Management

Conduktor allows you to manage and save the configuration and connection details to multiple Kafka clusters for easy and quick connections. The clusters you have used last will appear at the top of your cluster list. 

![the left-hand side contains the list of your clusters](../.gitbook/assets/image%20%286%29.png)

To edit the configuration of a cluster, hover your mouse over a cluster, and the "config button" will appear. 

![the config button appears next to the cluster name](../.gitbook/assets/image%20%2812%29.png)

{% hint style="info" %}
The ability to export, backup and share Kafka cluster configurations is an upcoming feature of the Enterprise License.  
{% endhint %}



## Secure Kafka Clusters

Setting up Conduktor with secure clusters is possible but requires you to have the exact properties. We have setup a longer guide here to help you out:

{% page-ref page="connecting-to-a-secure-kafka.md" %}

## Connectivity Issues

Having connectivity issues? Be sure that you're not in a case of "impossible connection setup". Read more here:

{% page-ref page="impossible-connection-setups.md" %}



