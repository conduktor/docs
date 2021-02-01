---
description: >-
  Defining the compatibility of your subjects ensure that your consumer will
  never have any issue when consuming your data, that you will never break them
  by mistake.
---

# Subjects & Schema Compatibilities

## Compatibilities

Long story short, there are 7 modes of compatibility handled by the Schema Registry:

* **Full + Transitive**: The safest. Add/Delete optional fields only. \[Recommended\]
  * Check compatibility with all the previous versions
* **Backward + Transitive**: Delete fields or Add optional fields only. Upgrade Consumers first.
  * Check compatibility with all the previous versions
* **Forward + Transitive**: Add fields or Delete optional fields only. Upgrade Producers first.
  * Check compatibility with all the previous versions
* **Full**:  Almost the safest. Add/Delete optional fields only.
  * Check compatibility only with the previous version
* **Backward**: Delete fields or Add optional fields only. Upgrade Consumers first.
  * Check compatibility only with the previous version
* **Forward**: Add fields or Delete optional fields only. Upgrade Producers first. Check compatibility only with the previous version
* **None**: Warning: no compatibility checking.
  * Be sure to know what you're doing

{% hint style="success" %}
**Transitive** check the whole lineage of schemas. Not transitive checks only the last 2 schemas.
{% endhint %}

You can see them in Conduktor, at the different places where you can change it \(global or per subject\):

![](../../.gitbook/assets/screenshot-2021-02-01-at-16.59.31.png)

## Resources

Confluent has a really [good article](https://docs.confluent.io/current/schema-registry/avro.html) about compatibility.



