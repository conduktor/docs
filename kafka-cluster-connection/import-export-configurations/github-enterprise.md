---
description: Use Github to store your cluster configuration privately among your team.
---

# GitHub \(enterprise\)

## How to share a cluster configuration with your teammates using Github

* As a Team Manager:

1. Export the configuration \(using export to JSON\)
2. Commit the file on a Github repository \(can be private\)
3. Share the github URL of the file to your teammates

   e.g.: `http://github.com/trobert/conduktor-configs/blob/main/config.json`

* As a developer:

Now for the teammates: On the welcome screen, create a new Apache Kafka cluster, then choose Github integration \(you need a enterprise subscription\):

![](../../.gitbook/assets/image%20%2840%29.png)

This will create a cluster in your Conduktor by fetching the configuration remotely, and keep it sync each time you connect to it.

### What if I have a Private Repository?

If your repository is private, you can easily create a Github token: The "Generate" button will bring you to a pre-filled github form to generate the token:

![](../../.gitbook/assets/image%20%2843%29.png)

All you need is to Click on "Generate Token" and copy/paste it in conduktor:

![](../../.gitbook/assets/image%20%2842%29.png)

## How to update a shared configuration

As a Team Manager, if you want to push a modification of the cluster configuration, just re-export the modified cluster configuration and commit the updated file on Github, at the same location.

Now, next time on of your team connect to this cluster, he will prompted to update his configuration:

![](../../.gitbook/assets/image%20%2844%29.png)





