---
description: >-
  Defining the compatibility of your subjects ensure that your consumer will
  never have any issue when consuming your data, that you will never break them
  by mistake.
---

# Subjects & Schema Compatibilities

## Compatibilities

Long story short, there are 7 modes of compatibility handled by the Schema Registry:

{% hint style="success" %}
**Transitive** means the whole lineage of schemas is verified. Not transitive means only the last 2 schemas are verified.
{% endhint %}

* **Full + Transitive**: The safest, recommended.
  * Add/Delete optional fields only. 
  * You can **ONLY** add optional fields \(basically, all your fields are optional!\): the consumer must explicitly handle the case each field is not present
  * _You can upgrade the Consumers or Producers like you want, no worries._
* **Backward + Transitive**
  * Delete fields or Add optional fields only.
  * _Upgrade Consumers first \(in case a mandatory field was deleted\)._
* **Forward + Transitive**:
  * Add fields or Delete optional fields only.
  * _Upgrade Producers first \(produce new mandatory fields before consumers\)_
* **Full**:  Almost the safest.
  * Add/Delete optional fields only.
  * You can **ONLY** add optional fields \(basically, all your fields are optional!\): the consumer must explicitly handle the case each field is not present
  * _You can upgrade the Consumers or Producers like you want, no worries._
* **Backward**
  * Delete fields or Add optional fields only.
  * _Upgrade Consumers first \(in case a mandatory field was deleted\)._
  * Check compatibility _only_ with the previous version
* **Forward**
  * Add fields or Delete optional fields only.
  * _Upgrade Producers first \(produce new mandatory fields before consumers\)._
  * Check compatibility _only_ with the previous version
* **None**: Be careful.
  * No compatibility checking. You can push incompatible schemas as you want.
  * Be sure to know what you're doing

You can see them in Conduktor, at the different places where you can change it \(global or per subject\):

![](../../.gitbook/assets/screenshot-2021-02-01-at-16.59.31.png)

## Resources

Confluent has a really [good article](https://docs.confluent.io/current/schema-registry/avro.html) about compatibility.



