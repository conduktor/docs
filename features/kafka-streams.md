---
description: >-
  Conduktor allows you to monitor your Kafka Streams applications and do the
  links with the existing topics, consumer groups etc. that the Kafka Streams
  application is using.
---

# Kafka Streams

{% hint style="info" %}
Work In Progress
{% endhint %}

## Import Topology

* Specify the application.id of your application
* Specify the topology
  * Static: paste your topology directly inside Conduktor
  * by URL: paste the endpoint of your application exposing its topology.
    * Conduktor will automatically fetch it regularly, adapt the metrics accordingly, and warn you if it's down

Here is an example importing a Kafka Streams application using the application.id `myapplicationid` and exposing a endpoint `/topology`:

![](../.gitbook/assets/screenshot-2020-09-20-at-18.56.14.png)

Conduktor will then monitor the endpoint and display a summary \(topics in and out\) in the main listing:

![](../.gitbook/assets/screenshot-2020-09-20-at-19.00.42.png)

If the application is down, the topology disappears and it becomes redish, time to call the developers!

![](../.gitbook/assets/screenshot-2020-09-20-at-19.02.14.png)



## How to reset a Kafka Streams Application?

Conduktor can help you in two ways:

* Do it manually by specifying all of topics in/out/internals/intermediate. It's not super practical.
* Do it with all topics already set because you imported the Topology within Conduktor already

![Manual \(specify everything\) or a registered application in Conduktor](../.gitbook/assets/screenshot-2020-09-20-at-19.05.04.png)

A wizard will then helps you by explaining the steps:

![](../.gitbook/assets/screenshot-2020-09-20-at-19.06.28.png)

If you're doing a reset of a registered Kafka Streams application within Conduktor, you can just hit Next until the end, everything is setup automatically! 🤩



