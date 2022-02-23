---
description: >-
  ACLs are nice, but very low-level and it's difficult to understand them.
  Conduktor provides a set of features to make it easier and also to detect any
  suspect security patterns.
coverY: 0
---

# ACLs Advanced Insights

ACLs are very useful to secure resources, but there are many different permissions to secure things (READ, WRITE, ALTER, CREATE, DELETE, and more…), it can be kinda complicated to understand easily what's going on at a glance.

Conduktor helps you visualize and manage your low-level ACLs, but also helps you by giving more high-level insights we'll describe below.

![Low-level ACLs, difficult to interpret](<../../.gitbook/assets/Screenshot 2022-02-20 at 20.39.06.png>)

## Topic / Users

Conduktor Enterprise offers a set of alternative views to inspect more easily with clear insights your ACLs:

* what a specific user can do regarding read/write/delete topics
* which ACLs on a User are useless (topics are gone for instance)
* expand the prefixed ACLs to the real topics of your clusters, this way you know exactly what is the impact of the ACL / on which resource

![](<../../.gitbook/assets/Screenshot 2022-02-20 at 20.51.50.png>)

Conduktor also provide an inverted view with all the topics and their associated users:

* how many users/who can read/write/delete each topic individually
* expand the prefixed ACLs to the real topics of your clusters, this way you know exactly all the users who have access to each particular topic

## Unsecured topics

Using Kafka, it's very important to be aware of the topics where no ACLs have been set.

According to your Kafka configuration, it can mean that ANY user can access them which is quite dangerous and probably not what you are looking for.

You have to be very careful of the value of the property "`allow.everyone.if.no.acl.found`". If "true", you're in trouble and all these topics are accessible by anyone. Consider setting it to "false"  to be sure you are not leaking data.

![](<../../.gitbook/assets/Screenshot 2022-02-20 at 21.20.21.png>)

### Public topics

It's important to know the topics accessible by any users (\*) on your clusters or by anonymous users. According to your data governance policy, having public topics can be a big problem, Conduktor makes it easier to identify and fix them.

![](<../../.gitbook/assets/Screenshot 2022-02-20 at 21.05.40.png>)















