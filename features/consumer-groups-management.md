---
description: >-
  Consumer Groups are one of the most powerful concept in Apache Kafka. It is
  used to track the position of each consumer. In case it goes offline, it can
  resume from its last position.
---

# Consumer Groups Management

{% embed url="https://www.youtube.com/watch?v=RCpjLuZyPq4&list=PLYmXYyXCMsfMMhiKPw4k1FF7KWxOEajsA&index=14&ab_channel=ConduktorConduktor" %}

## Conduktor & Consumer Groups

Conduktor does NOT change offsets of any Consumer Groups when consuming data, it does not need too. Therefore, it's totally transparent for your Kafka cluster: Conduktor won't leave any traces and will not create anything here, you're safe.

## How can I create a Consumer Group?

_TLDR: you can, but you don't need to._

Conduktor can be used to create a new Consumer Group, by selecting either one or multiple topics from the main Topics view and using the action to create a consumer group. It can be customized by setting the initial offsets for the selected topic(s) to either Earliest of Latest. This feature should be used only in corner-cases where you require to configure a pre-existing Consumer Group for your application.

For any other functionality, Conduktor does not create any Consumer Groups. Ideally, the Consumer Group should be created by your applications, to be used by themselves.

## How to delete a Consumer Group?

* **Stop the applications** running on this Consumer Group, otherwise you won't be able to delete it. ⚠️
* Two ways to delete a Consumer Group in Conduktor
  * from the main list
  * from the Consumer Group details

![](../.gitbook/assets/screenshot-2020-09-20-at-12.21.07.png)

## How to Reset the Offsets of a Consumer Group?

* **Stop the applications** running on this Consumer Group, otherwise the buttons won't be accessible. Don't worry, we clearly state that the group must be stopped before acting on it 😉.

![](../.gitbook/assets/screenshot-2020-09-20-at-12.03.06.png)

* Click on "Change offsets" then configure the strategy to how reset the offsets:
  * which topic: a consumer group can subscribe to multiple topics
  * which partition: sometimes you know exactly which message is stuck!
  * the new offset. It's computed automatically according to a few methods:&#x20;
    * **Earliest**: start from the beginning, reprocess everything!
    * **Latest**: start from the end, ignore all the existing records
    * **Latest minus X**: just reprocess the latest X records
    * **Current offset + X**: to jump over some records
    * **Specific Datetime**: when you know when your app started to crash
    * **Current time minus X seconds**: just the latest records in time
* When you change the strategy, Conduktor automatically recomputes the **New Offset** (right column).
* Click on `CHANGE OFFSETS (X)` when you're happy with the proposal
  * X is the number of changes
* Don't forget to restart your application! 💪

![](../.gitbook/assets/screenshot-2020-09-20-at-12.06.59.png)

## How can I track the lag of my Consumer Group and my topics?

### Getting the lag for a Consumer Group

The global view of the Consumers shows the overall lag of your consumer groups. It is the sum of the lag for all topic-partitions the consumer group is subscribing to. When it's empty, it means there is no lag, and it's a good news!

* **The overall lag** is the sum of the **differences** between the latest offsets committed by the consuming applications for all their partitions, and the current end offset of all the partition where records are being written to.
* **Members**: how many instances of your applications are using this consumer group
* **Partitions / Topics**: how many partitions your consumer group is keeping track of

![](../.gitbook/assets/screenshot-2021-01-29-at-19.23.34.png)



### Getting the lag per partition

You can also see the details per partitions in the details. Note that a consumer group can subscribe to multiple topics, like we can see below.

* **"?" means indeterminate**.
  * The Consumer Group has not committed yet any offset for this partition. This can happen when you don't have any record in this specific partition for instance (or if you have a bug in your consuming application!).
  * Sometimes, it can happen when Conduktor has troubles to simply get the offsets because the Kafka cluster is slow to respond, therefore Conduktor timeouts after a while (30 seconds by default).
  * The Current Offset of first three partitions below are also "?" because no commit were made yet, hence the lag is indeterminate.
* The lag is the **difference** between the latest offset committed by the consuming applications for the partition, and its current end offset where records are being written to.

![](../.gitbook/assets/consumer-group-question-mark.jpg)

## How to force a Member to leave a Consumer Group

You can **not** force a consumer to leave a consumer group _permanently_.&#x20;

However, Conduktor allows you to remove members from a group, which will have for effect to trigger a group rebalance. This is helpful if a member is dead, but its partitions are not yet reallocated to another member. Active members will re-join the group automatically during the rebalance and the partitions will be reassigned.

There are two ways to remove members from a consumer group:

* Remove all members at once with the `Remove All Members And Rebalance` button.
* Remove a selection of members : select the members and use the `ACTIONS` menu. This is only available for member of type static.

{% hint style="success" %}
Designed to be used with the **static group membership** feature_,_ an Apache Kafka Consumer feature. It delays rebalancing operations when a member is temporary gone (pod restart, rolling upgrade..) to prevent useless rebalancing.

BUT now, the consumer group coordinator can take time to realize a static member is gone, so forcing a kick here will help trigger rebalancing as soon as possible, to reassign partitions to other members.
{% endhint %}

