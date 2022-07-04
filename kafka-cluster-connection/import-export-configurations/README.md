# Import/Export configurations

This feature is for **Enterprise** users where this is typically needed, to share your configuration with your team. You can also use [Github](github-enterprise.md) to auto-synchronize your configuration with your team.

### Export a Cluster Configuration

On the welcome screen, you can export your configuration to a file (cluster-name.json).

![](<../../.gitbook/assets/1-export (1).png>)

### Import a Cluster Configuration

You can import a Cluster Configuration from a file or directly from your clipboard! If this cluster did not exist yet in your Conduktor, it will create it directly. Otherwise, it will ask you if you want to override the existing cluster configuration or not (see below).

![](<../../.gitbook/assets/2-import (1).png>)

### Overriding or Copying an existing cluster configuration

Conduktor will detect if you already have the cluster configuration and will ask you what to do. This can happen when you clone a configuration of yours for instance, or if you just want to update a cluster configuration that you got from someone.

![](<../../.gitbook/assets/3-override (1).png>)

### What the import/export does copy

The import/export is only about the configuration, not your personal preferences, which topics or consumer groups you've favorited, your producer templates etc. It's only about the cluster configuration (Apache Kafka, Kafka Connect, Kafka Streams, ksqlDB, Schema Registry, Metrics...)
