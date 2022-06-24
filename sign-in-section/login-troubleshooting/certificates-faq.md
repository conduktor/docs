---
description: >-
  This page will guide you on all certificate-related issues that may occur when
  using Conduktor such as during Conduktor Login or while connecting to Kafka
  brokers, Kafka Connect, Schema Registry, ...
---

# Certificates FAQ

### Introduction

If your organization deploys Kafka services securely (with TLS) with **non-trusted issuers** or uses **self-signed certificates**

or

If your organization is using an internet **proxy with SSL Inspection** feature enabled such as

* ZScaler
* PaloAlto Networks
* Symantec BlueCoat

you might run into problems at different stages of Conduktor:

* During Conduktor Login phase
* While connecting to your Kafka cluster
* While connecting to your other Kafka services (Connect, Registry, ...)

Whatever your scenario, don't worry, this page will help you get everything sorted out in no time.

### Identify a certificate-related issue (PKIX path building failed)

Certificate errors typically come with the following error message:

```
org.apache.kafka.common.errors.SslAuthenticationException: SSL handshake failed 
Caused by: javax.net.ssl.SSLHandshakeException: PKIX path building failed: 
sun.security.provider.certpath.SunCertPathBuilderException: 
unable to find valid certification path to requested target
```

To fix this, you have to let Conduktor know that your company's certificate is safe to use.

We have implemented a new feature to disable certificate validation in Conduktor in order to help you troubleshot certificate-related issues.

On the Cluster selection screen, navigate to _Options > Network > Trusted Certificates_

Check the box "Allow non-trusted certificates automatically"

![](../../.gitbook/assets/network-tab.png)

![](../../.gitbook/assets/ignore-ssl.png)

Restart Conduktor and try again.&#x20;

If your errors disappear, you can either leave the box checked or resolve your certificate issue properly by importing them.

### Importing certificates in Conduktor

If you already have the certificates at your disposal, you can directly import them in the Trusted Certificates tab from above.

PEM certificates looks like this (generally .pem or .crt files)

```
-----BEGIN CERTIFICATE-----
MIIEZTCCA02gAwIBAgIQQAF1BIMUpMghjISpDBbN3zANBgkqhkiG9w0BAQsFADA/
MSQwIgYDVQQKExtEaWdpdGFsIFNpZ25hdHVyZSBUcnVzdCBDby4xFzAVBgNVBAMT
[...]
O5b85o3AM/OJ+CktFBQtfvBhcJVd9wvlwPsk+uyOy2HI7mNxKKgsBTt375teA2Tw
UdHkhVNcsAKX1H7GNNLOEADksd86wuoXvg==
-----END CERTIFICATE-----
```

If you don't have them, get in touch with your Administrator to help you.

### Conduktor Certificate model

We are pleased to present to you our new Certificate model, intended to simplify your life as much as possible when your architecture involves **non-trusted issuers.**

![The SSL Context is shared across all connections except ksqlDB](../../.gitbook/assets/ssl-context.png)

With this new model, you just need to import the certificates once in the Network tab and they will be picked up by all the components.&#x20;

If you are using Windows and Mac, and your organization has pushed certificates into your machine using GPO, we will even load them!&#x20;

This means that you don't need to define the trust store for each individual component anymore.&#x20;

{% hint style="danger" %}
Unfortunately, we were not able to integrate **ksqlDB** in our new certificate model yet.
{% endhint %}

We remain fully compatible with the previous methods so if you have configured your Kafka clusters with .jks files, or if you still want to use this mechanism, everything will work as usual.

#### Additional properties **(for Kafka brokers and Schema Registry)**

```
ssl.truststore.location=/path/to/truststore.jks
ssl.truststore.password=changeit
```

#### JKS Manual Import (for Kafka Connect and ksqlDB)

![The previous method is still available](../../.gitbook/assets/manual-jks.png)

### Client Certificate Authentication

If you are using Client Certificate Authentication to connect to your Kafka Clusters, you have 2 possibilities to successfully connect to your Kafka Cluster

#### Keystore using JKS file

```
ssl.keystore.location=/path/to/keystore.jks 
ssl.keystore.password=changeit
ssl.key.password=test1234
```

#### Inline Access Key and Access Certificate

{% hint style="warning" %}
Please note the trailing `\` at the end of each line which signals a multiline content
{% endhint %}

```
ssl.keystore.type=PEM 

ssl.keystore.key=-----BEGIN PRIVATE KEY----- \
MIIG/gIBADANBgkqhkiG9w0BAQEFAASCBugwggbkAgEAAoIBgQDMMRR0fQSli2tX \
Ip9QIvB7ef4xzUh3qGCoBsgqLubkgkEdauznHtn4EuDkhbKbWjUg37WMS4TjtUYt \
OHwT1aX5LrgKL1vYxzzOBAk46oUiboIv1PMbVS5LmO4ALtjR1N1rLL+OWVwuDCd/ \
1+T//b58rImpt5Ah2E1TYDK3 \
-----END PRIVATE KEY----- 

ssl.keystore.certificate.chain=-----BEGIN CERTIFICATE----- \
MIIEPzCCAqegAwIBAgIUX6T5U95harasueNwvjRgspnkJT0wDQYJKoZIhvcNAQEM \
BQAwOjE4MDYGA1UEAwwvNzAzMDg5NzYtNjU2YS00ZDkzLThkZGQtMzJiZGNmYmM2 \
6mpaK0/FlLZoV7WXcvgYLxHA6t249YI2cjTS+FXfBgFtXyF/gLsse26HCs/vpCB6 \
LsQbSqSb9md+ToDyp/wHCspeWkWJTYu4YZUmkR2dhFvDJbhANIXwICQc18dsVEDl \
vWv09PuooDhAdBZi0UsBa0XUwvzKFlZfaJ7csh20jvR7Vpg= \
-----END CERTIFICATE-----
```
