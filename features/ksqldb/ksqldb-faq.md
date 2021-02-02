---
description: Some issues and questions you may have using ksqlDB in Conduktor
---

# ksqlDB FAQ

## Not a query error?

Conduktor only supports streams queries in its editor \(CREATE STREAM, CREATE TABLE etc.\). You cannot use the metadata queries such as "SHOW STREAMS", it's not made for this.

Conduktor already handles all the metadata stuff on its own and displays them properly in its interface.

## Compatibility with Conduktor \(40400\)

Conduktor only fully support ksqlDB from version 0.10.x. Most things works with older versions except the ksqlDB queries \(you may end up with errors like _KsqlClientException: Received 404 response from server: HTTP 404 Not Found. Error code: 40400_\)

If you're using Docker, double-check which Docker image you're using. Confluent Platform 5.5.1 is only using ksqlDB version 0.7.1 which is very old \(many stuff changed since\), you can refer to their compatibility matrix here: [https://docs.confluent.io/current/installation/versions-interoperability.html\#ksqldb](https://docs.confluent.io/current/installation/versions-interoperability.html#ksqldb). ksqlDB 0.10.x will be supported only for Confluent Platform 6.x.

If you encounter an issue, you may instead use the Docker image `confluentinc/ksqldb-server:0.11.0` as described in the official quickstart: [https://ksqldb.io/quickstart.html](https://ksqldb.io/quickstart.html)

