# Starting a local Kafka cluster in seconds

Starting a Kafka cluster locally could be really painful. Each operating system has their own particularities. Some technologies like Docker provide solutions for managing processes, but it isn't installed by default and doesn't work out of the box on Windows.

For this reason, Conduktor offers you an easy way to start a Kafka cluster. This feature is cross-platform (Linux, macOS and Windows) and works out of the box. You don't need to install anything but Conduktor.

## How does it work?

{% embed url="https://youtu.be/CHyl5jPVqV4" %}

Click on "Start a local Kafka cluster", then you can configure your cluster :

![](../.gitbook/assets/2022-03-09\_17-39.png)

* Choose a Kafka version&#x20;
* Do you need a registry? Just activate the toggle.&#x20;
* Start your cluster!

The cluster will be entirely managed by Conduktor. You don't have to configure anything else, just enjoy the experience.

Once you are connected to your managed Cluster, you can copy and paste the addresses of your Kafka cluster and registry to use it outside of Conduktor :&#x20;

![](<../.gitbook/assets/image (53) (1).png>)

## Common error cases

### 1 - Process exited unexpectedly

One of the Kafka processes failed to start or end suddenly. You can try again, if the issue persists, check the log and/or contact the support team!

### 2 - Process timeout

The process failed to start for an unknown reason. You can try again, if the issue persists, check the log and/or contact the support team!

### 3 - Node exists

Kafka process abort suddenly in previous run, the previous node id is still registered in Zookeeper and so the start failed. This error should not happen, if so, please contact the support team.

### 4 - Lock file already held

Another Kafka process is already running on the current directory. It could happen if you have another Conduktor desktop instance that runs this cluster. Otherwise, your cluster has maybe failed without releasing the lock. Unfortunately, the only solution is to clean your cluster data directory.
