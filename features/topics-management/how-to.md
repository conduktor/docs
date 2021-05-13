# How to

## How Conduktor helps when Kafka is failing?

{% embed url="https://www.youtube.com/watch?v=ZJTw2-GVr-4&list=PLYmXYyXCMsfMMhiKPw4k1FF7KWxOEajsA&index=17&ab\_channel=ConduktorConduktor" %}

## How to consume data from a Topic?

Several ways:

* from the top bar: add a Consumer and select your topic inside
* from the topic list: the inspect button next to the name of the topic
* from the topic details view: from the _CONSUME DATA_ button

Also see [Consuming data ](../consuming-data/)for all the details.

## How to reset the offsets of a Topic?

The question is more: _how to reset the offsets for a Consumer Group and a Topic it has subscribed to?_

The offsets of a Topic cannot be "reset". The offsets are ever growing for all partitions of a Topic \(data incoming into a Topic =&gt; offsets increases\). 

Go to the Consumer Group itself, and Reset Offsets from here.

## How to reassign the partitions of a Topic?

In the Topic Details view, click on the "Advanced" drop-down, then choose "Reassign partitions..." to open the Reassignment dialog.

![](../../.gitbook/assets/topic-reassign-dropdown.png)

Choose the destination brokers, pick a strategy and Conduktor will generate the Reassignment Plan for you.  
You can also choose to import your own in the json format expected by kafka-reassign-partitions.sh.

![Partition Reassignment dialog](../../.gitbook/assets/reassignment-not-started.png)

You can decide to automatically elect partition leaders after the reassignment.

The throttle limit is an important option in production : without it, the reassignment may end up using all the available bandwidth, degrading your cluster performance. You can alter this configuration even when a reassignment is in progress, it will take effect immediately.

When a reassignment is in progress, you can monitor it from anywhere using the topbar button : 

![](../../.gitbook/assets/current-reassignment-topbar.png)

The opened dialog allows you to check the reassignment status and update the throttle limit.

## How to add partitions to a Topic?



## How to remove data from a Topic?

Conduktor can empty a topic, or a specific partition:

* a whole topic: "Empty Topic..."

![Empty a topic](../../.gitbook/assets/screenshot-2021-02-23-at-18.30.22.png)

* a specific partition: the Delete icon on the right

![](../../.gitbook/assets/screenshot-2021-02-23-at-18.31.05.png)

### Errors

You can only delete topic which have the "delete" policy.

If your topic only has the "compact" policy, this may not work: 

```text
Error emptying topics

org.apache.kafka.common.errors.PolicyViolationException:
Request parameters do not satisfy the configured policy.
```

