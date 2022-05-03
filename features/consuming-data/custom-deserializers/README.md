---
description: >-
  Consume your data from Conduktor even if Conduktor doesn't support the format
  you used to serialize it.
---

# Custom deserializers

> Available since v2.19.0

Conduktor supports natively all the data formats natively supported by Apache Kafka.\
Some examples of these natively supported formats are:

* String, Int, Double, Boolean, etc.
* JSON
* JSON from a SchemaRegistry
* Avro
* Avro from a SchemaRegistry
* Protobuf (since v2.21.2)
* Protobuf from a SchemaRegistry
* etc.

But it may not be sufficient to you.\
You may have some data in your Kafka cluster(s) encoded using a different format.\
You could have for example used [MessagePack](https://msgpack.org/index.html), [Capt'n Proto](https://capnproto.org), or even your own custom serialisation format that you developed internally to encode your data in Kafka.

To allow you to consume this data serialized with a "custom format" directly from Conduktor, Conduktor allows you to use your own custom deserializers.

This page will explain you how to configure Conduktor to use your custom deserializer to consume this kind of data.

### Step 1: Add your custom deserializer(s) into your cluster configuration

For that step, we will use the "Plugins" capability of Conduktor.

_You can have more information on this "Plugins" feature in the_ [_Plugins documentation_](../../../kafka-cluster-connection/setting-up-a-connection-to-kafka/)

A Kafka deserializer is an implementation of the `org.apache.kafka.common.serialization.Deserializer<T>` Java interface (for more information, see [kafka-clients documentation](https://kafka.apache.org/30/javadoc/org/apache/kafka/common/serialization/Deserializer.html)).\
You need to have one, or more, jar(s) containing these implementations so you can add these jar files in the "Plugins" section of your cluster configuration in Conduktor.

_You can find some Kafka deserializer implementation examples in this open-source Github repository:_ [_my\_custom\_deserializers_](https://github.com/conduktor/my\_custom\_deserializers)\
_In the README of this project, a link to download a `.jar` file containing these Kafka deserializers is provided so you can test the feature with them._\
_The behaviour of each Kafka deserializer implementation is explained in the README._

![](../../../.gitbook/assets/custom\_deserializer/add\_custom\_deserializer.gif)

⚠️ Plugins are part of a cluster configuration. When you add a plugin to one of your cluster configuration, this plugin is only available to this cluster.\
If you want to use the same plugin plugin with another cluster configured in Conduktor, you'll need to add this plugin to the configuration of this other cluster too.

### Step 2: Select your topics, your deserializer and consume your data

Now that you added your plugin(s) containing your custom deserializer(s) to your cluster configuration, connect to your cluster, and open the "Consumer" windows.

In the "Consumer" windows, select the topic you want to consume, then in "Format" sub-menu, in the "value" format selector, choose the "Custom Format" option.\
This will open a new sub-menu named "Custom Format: Configuration".\
In this new sub-menu, there are two fields:

* a dropdown allowing you to select your custom deserializer implementation class
* a textarea field allowing you to pass some properties to your custom deserializer implementation (We'll call the `org.apache.kafka.common.serialization.Deserializer<T>::configure` method with these properties)

Last thing to do is to hit the "Start" button to start the consumption of your selected topic data 🎉

![](../../../.gitbook/assets/custom\_deserializer/use\_custom\_deserializer.gif)
