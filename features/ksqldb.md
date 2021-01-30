---
description: Streaming everything with ease!
---

# ksqlDB: Streaming with SQL

Conduktor only fully supports ksqlDB from v0.10.x and was tested until v0.14. Don't hesitate to contact us if you stumbled upon some issues. ksqlDB is still in development \(&lt;1.0\), so they may break things for the sake of evolution. We try our best to be compatible with all versions.

## How to start with Confluent Cloud?

You need to generate an API key and secret to act as username and password specific for ksqlDB. Do not use the Kafka credentials you should already got, this won't work. This can be done using Confluent Cloud CLI tool: `ccloud`. Install it using the official documentation: [https://docs.confluent.io/ccloud-cli/current/install.html](https://docs.confluent.io/ccloud-cli/current/install.html)

* Login and select your environment if you have several of them:

```text
$ ccloud login
$ ccloud environment
```

* Grab your `<ksql-cluster-id>` and its endpoint:

```text
$ ccloud ksql app list
       Id      |     Name     | Topic Prefix |   Kafka   | Storage |                         Endpoint                          | Status
+--------------+--------------+--------------+-----------+---------+-----------------------------------------------------------+--------+
  lksqlc-0yp12 | ksqlDB_app_0 | pksqlc-1ymox | lkw-o1yvz |     500 | https://pksqlc-1ymox.europe-west0.gcp.confluent.cloud:443 | UP
```

* Generate a key: write down the key & secret:

```text
$ ccloud api-key create --resource lksqlc-0yp12
It may take a couple of minutes for the API key to be ready.
Save the API key and secret. The secret is not retrievable later.
+---------+------------------------------------------------------------------+
| API Key | ABCDEFKZBF56666                                                  |
| Secret  | ToMaHaWkjQ1bt7BxvdyFjaJ8j3nSokaAd83Nhan739snAiufIAfdk7fFAAnBKxai |
+---------+------------------------------------------------------------------+

# you can list all the keys (without secrets)
$ ccloud api-key list --resource lksqlc-0yp12
```

* The API Key is the username, the Secret is the token

Configure your Conduktor with all these elements, selecting Basic Auth to add the username/password:

![](../.gitbook/assets/screenshot-2021-01-29-at-00.02.42.png)

Test for the Connectivity here, to ensure the API Key is ready, it may takes a few minutes!

## Compatibility with Conduktor \(40400\)

Conduktor only fully support ksqlDB from version 0.10.x. Most things works with older versions except the ksqlDB queries \(you may end up with errors like _KsqlClientException: Received 404 response from server: HTTP 404 Not Found. Error code: 40400_\)

If you're using Docker, double-check which Docker image you're using. Confluent Platform 5.5.1 is only using ksqlDB version 0.7.1 which is very old \(many stuff changed since\), you can refer to their compatibility matrix here: [https://docs.confluent.io/current/installation/versions-interoperability.html\#ksqldb](https://docs.confluent.io/current/installation/versions-interoperability.html#ksqldb). ksqlDB 0.10.x will be supported only for Confluent Platform 6.x.

If you encounter an issue, you may instead use the Docker image `confluentinc/ksqldb-server:0.11.0` as described in the official quickstart: [https://ksqldb.io/quickstart.html](https://ksqldb.io/quickstart.html)



