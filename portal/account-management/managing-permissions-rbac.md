---
description: >-
  Allow/Deny access to specific features of Conduktor Desktop for sensitive
  production clusters.
---

# Managing Permissions / RBAC

{% hint style="info" %}
This feature is available only for Enterprises.
{% endhint %}

To be in control of what features your users can use on your clusters when using Conduktor Desktop, considering limiting their permissions. This is available on your account at [https://account.conduktor.io](https://account.conduktor.io/).

## Scope

Today, you can control this on a **per-cluster** basis.

## Example

You want to ensure that all your users cannot write into your production clusters: you want to provide read-only access. You also want them to have all the permissions on your development clusters: this is the default (when no restrictions are put in place).

## How to control the permissions of an Apache Kafka cluster?

* On the Account Management Portal, click on the **Manage** button under Subscription Details > Cluster Permissions to get to the _Cluster Permission_ page :

![](../../.gitbook/assets/image-2-.png)

* You will be able to declare your clusters or change the permission of an existing cluster :&#x20;

![](<../../.gitbook/assets/Screenshot 2021-12-17 at 16-52-59 Conduktor Customer Portal.png>)

* Click **Manage a new cluster** to declare a new cluster, you need to provide:
  * the Cluster ID: provides by your Apache Kafka administrators / your Ops team / Also available within Conduktor in the "Brokers" view
  * a short name
  * a longer description.&#x20;

![](../../.gitbook/assets/capture-decran-du-2021-08-26-16-07-44.png)

{% hint style="success" %}
A cluster ID uniquely identifies your clusters. It is a unique identifier assigned automatically to an Apache Kafka cluster.&#x20;
{% endhint %}

## How to setup the permissions of a cluster?

Once you've added a cluster, you will be able to allow or deny access to specific features of Conduktor Desktop. Click on button **Access Control** on the right of your created cluster.

By default, there is any restriction on what your users can do on your clusters. The access control is disabled :&#x20;

![](<../../.gitbook/assets/Screenshot 2021-12-17 at 16-58-09 Conduktor Customer Portal (1) (1).png>)

{% hint style="success" %}
You can enable access control and choose the base role _Viewer_ to set predefined permissions to prevent any modifications to your cluster by your users (eg: disallowing producing to topics, broker config modifications, topic creation/deletion, altering schemas in schema registry, ...)​
{% endhint %}

## How to set a role for a specific user?

You can provide different permissions set for your users by assigning them different roles. You have to enter the user mail, click on the button **Add user**, and assign to him the desired role(s) :&#x20;

![](<../../.gitbook/assets/Screenshot 2021-12-17 at 17-07-23 Conduktor Customer Portal.png>)

{% hint style="warning" %}
The base role will give its permissions access to every users of the subscription. You can't give less access right for a single user. We advise you to not give a too powerful base role on your cluster.
{% endhint %}

## Could I define my own role?

We predefined three roles to help you restrict access :&#x20;

* Viewer : You can only read data on the cluster.
* Editor : You can write data on the cluster.
* Owner : You have access to all the features available.

But you can define your own role on the **Roles** tab with the **Add a custom role** button :

![](<../../.gitbook/assets/image (51).png>)

## How does this affect Conduktor Desktop?

When a permission is denied, the corresponding button or menu entry will not appear in Conduktor Desktop.

For instance, if we disabled "Produce Into Topics" on a cluster, users connected to this cluster won't be able to Produce any data (no "Producer" button):

![Notice the lack of some buttons (⊕ producer , import data, ...)](../../.gitbook/assets/capture-decran-du-2021-08-26-17-43-22.png)

![Same view with all permissions enabled](../../.gitbook/assets/capture-decran-du-2021-08-26-17-42-03.png)

{% hint style="info" %}
Removing the specific permission "_Allow Users to Connect_" will entirely prevent the connection to the cluster.
{% endhint %}

## Additional Remarks

* This does not persist any ACLs into your Apache Kafka clusters, this controls only Conduktor
* Clusters that are not declared in the portal get all permissions by default.

