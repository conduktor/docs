---
description: >-
  Allow/Deny access to specific features of Conduktor Desktop for sensitive
  production clusters.
---

# Managing Permissions / RBAC

{% hint style="info" %}
This feature is available to **Billing users** and **License Managers** only.
{% endhint %}

You can have control of what features of Conduktor Desktop is available to users in your subscription. This control is on a **per-cluster** basis. For example, you can give a read-only access to your production clusters, while still allowing full access to you development clusters.

## Defining a Kafka Cluster 

On the Account Management Portal, click on the **Manage** button under Subscription Details &gt; Cluster Permissions to get to the _Cluster Permission_ page :

![](../../.gitbook/assets/image-2-.png)

From the next page, you will be able to add a new cluster or change the permission of an existing cluster:

![](../../.gitbook/assets/capture-decran-du-2021-08-26-15-54-47.png)

Click C**reate** to add a new cluster, you will be asked with a Cluster ID, name and description. 

![](../../.gitbook/assets/capture-decran-du-2021-08-26-16-07-44.png)

{% hint style="info" %}
The Cluster ID is the important information that allows Conduktor Desktop to retrieve the permissions of the clusters.

**You can find it in Conduktor Desktop in the Broker Tab**
{% endhint %}

![Where to find the cluster ID in Conduktor Desktop](../../.gitbook/assets/capture-decran-du-2021-08-26-17-05-01.png)

## Settings the permissions of a cluster

Once you added a cluster, you will be able to allow or denying access to specific features of Conduktor Desktop. Check the permissions you want to allow :

![](../../.gitbook/assets/capture-decran-du-2021-08-26-16-09-46.png)

{% hint style="info" %}
You can click _Read-only_ to get a preset of permissions which will prevent modifications to your cluster \(disallowing producing to topics, broker config modifications, topic creation/deletion, altering schemas in schema registry, ...\)
{% endhint %}

##  Effects of permissions on Conduktor Desktop

If a permission is denied, the corresponding button or menu entry will not appear in Conduktor Desktop. For instance, you can only create consumers but not producers on this cluster:

![Read-only permissions: Notice the lack of some buttons \(&#x2295;producer , import data, remove ...\)](../../.gitbook/assets/capture-decran-du-2021-08-26-17-43-22.png)

![Same view with all permissions enabled](../../.gitbook/assets/capture-decran-du-2021-08-26-17-42-03.png)

{% hint style="info" %}
Removing the specific permission "_Allow Users to Connect_" will totally prevent the connection to the cluster.
{% endhint %}

## Additional Remarks

* The restrictions only apply when using to Conduktor Desktop. It still possible to by-pass the limitation using \(for instance\) the classical Kafka clients.
* Refining permission on a per-user basis will be available in a future version of the product.
* Clusters which are not referenced in the portal will get all permissions by default.



