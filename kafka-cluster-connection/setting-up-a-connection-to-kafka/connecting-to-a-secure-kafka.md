---
description: >-
  Connecting to a secure Kafka cluster may be difficult, but hopefully, this
  page will help you. Conduktor inherits the permissions of the user that
  connects to Kafka.
---

# Connecting to a Secure Kafka

## YouTube video walkthrough

{% embed url="https://youtu.be/_NQmMUQL07Y" %}

## General Idea

Conduktor leverages the default Apache Kafka Java Clients, and therefore we use the same [configuration properties](https://kafka.apache.org/documentation/#consumerconfigs). **If you are trying to connect to a secure Kafka cluster using Conduktor, please first try to use the CLI.** If you don't know how, please contact your administrator.&#x20;

**Example:**

```bash
kafka-console-consumer \
    --topic my-topic \
    --bootstrap-server SASL_SSL://kafka-url:9093 \
    --consumer.config config.properties
```

Your `config.properties` file may contain something like this:

```
security.protocol=SSL
ssl.truststore.location=/var/private/ssl/client.truststore.jks
ssl.truststore.password=test1234
other.configs=values...
```

What Conduktor needs to connect to a secure Kafka cluster is all the values from your `config.properties` file.

{% hint style="info" %}
In case you don't know what should be the values in the `config.properties` file, please contact your Kafka administrator.&#x20;

**Note:** these are the same properties you would use in your Kafka Java clients or applications.&#x20;
{% endhint %}

## SSL Configuration

If client authentication is not required by the broker, the following is a minimal configuration example:

```
security.protocol=SSL
ssl.truststore.location=/full/path/to/kafka.client.truststore.jks
ssl.truststore.password=test1234
```

If client authentication is required, then a keystore must be created for each client, and the brokers’ truststores must trust the certificate in the client’s keystore. Please ask your Kafka administrator for help on generating client keys. Here is a configuration example:

```
security.protocol=SSL
ssl.truststore.location=/full/path/to/kafka.client.truststore.jks
ssl.truststore.password=test1234
ssl.keystore.location=/full/path/to/kafka.client.keystore.jks
ssl.keystore.password=test1234
ssl.key.password=test1234
```

Other configuration settings that may also be needed depending on our requirements and the broker configuration:

1. ssl.provider (Optional). The name of the security provider used for SSL connections. Default value is the default security provider of the JVM.
2. ssl.cipher.suites (Optional). A cipher suite is a named combination of authentication, encryption, MAC and key exchange algorithm used to negotiate the security settings for a network connection using TLS or SSL network protocol.
3. ssl.enabled.protocols=TLSv1.2,TLSv1.1,TLSv1. It should list at least one of the protocols configured on the broker side
4. ssl.truststore.type=JKS
5. ssl.keystore.type=JKS

{% hint style="warning" %}
Please make sure to put the **full paths** to your SSL certificates in your properties file\
Relative paths may not work for Conduktor.&#x20;
{% endhint %}

## SASL Configuration

Multiple SASL configurations can be done for Apache Kafka and Conduktor supports them all. In this documentation we will just cover Kerberos, but you should get a general sense of how things work.&#x20;

Here's a minimal configuration for SASL\_PLAINTEXT:

```
security.protocol=SASL_PLAINTEXT
sasl.mechanism=GSSAPI
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="my-user" password="secret";
```

### SCRAM configuration

Use this configuration in Conduktor:

```
security.protocol=SASL_SSL
sasl.mechanism=SCRAM-SHA-512
sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required username="yyy" password="xxx";
```

### 🚨 About JAAS files

If you see a JAAS file being passed as a Java option to your Kafka clients using

```
-Djava.security.auth.login.config=/etc/kafka/kafka_client_jaas.conf
```

then you must you the `sasl.jaas.config` property as outlined above in Conduktor.

**Example:** the following JAAS file:

```
KafkaClient {
  org.apache.kafka.common.security.plain.PlainLoginModule required
  username="alice"
  password="alice-secret";
};
```

Would be converted to the following `sasl.jaas.config` property:

```
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required  username="alice" password="alice-secret";
```

{% hint style="warning" %}
Please make sure to put the **full paths** to your SASL key files in your properties file\
Relative paths may not work for Conduktor.&#x20;
{% endhint %}

### Example using Kerberos

Another example using Kerberos and a keytab:

* a JAAS file (would need -Djava.security.auth.login.config=/path/to/jaas.conf)

```
KafkaClient {
  com.sun.security.auth.module.Krb5LoginModule required
  useKeyTab=true
  keyTab="/etc/security/keytabs/alice.keytab"
  principal="alice@EXAMPLE.COM";
};
```

* The same, but using `sasl.jaas.config`:

```
sasl.kerberos.service.name=kafka
sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required useKeyTab=true keyTab="/etc/security/keytabs/alice.keytab" principal="alice@EXAMPLE.COM";
```

****

**Troubleshooting  : " KrbException: Pre-authentication information was invalid** " **ERROR**\


* **Cause 1:** The password entered is incorrect.
  * _**Solution 1:** Verify the password._
* **Cause 2**: If you are using the keytab to get the key (e.g., by setting the `useKeyTab` option to`true` in the Krb5LoginModule entry in the JAAS login configuration file), then the key might have changed since you updated the keytab.
  * _**Solution 2:** Consult your Kerberos documentation to generate a new keytab and use that keytab._
* **Cause 3**: Clock skew - If the time on the KDC and on the client differ significantly (typically 5 minutes), this error can be returned.
  * _**Solution 3:** Synchronize the clocks (or have a system administrator do so)._
* **Cause 4**: The Kerberos realm name is not all uppercase.
  * _**Solution 4:** Make the Kerberos realm name all uppercase. Note: It is recommended to have all uppercase realm names. See_ [_Naming Conventions for Realm Names and Hostnames_](https://docs.oracle.com/en/java/javase/11/security/kerberos-requirements.html#GUID-E73CCEA1-E94F-4E8D-9C42-403AF825658A)_._\


## FAQ

### Ensure you are using the Java-style configuration

If you are sure you have configured your connection properly in Conduktor and it works in other tools, make sure you're using the official configuration Java-style, and not the C-style (librdkafka). It may happen when you work with Python or nodejs (both are using librdkafka behind the scene).&#x20;

[Here are all the properties of librdkafka](https://docs.confluent.io/5.3.2/clients/librdkafka/md\_CONFIGURATION.html), some of them are different from the [official Java configuration](https://docs.confluent.io/platform/current/installation/configuration/admin-configs.html) (that Conduktor supports).

For instance, the following (common) properties are NOT compatible with Conduktor:

```
sasl.username
sasl.password
enable.ssl.certificate.verification
```

You need to use the Java-style syntax shown above, here with SASL\_PLAINTEXT:

```
security.protocol=SASL_PLAINTEXT
sasl.mechanism=GSSAPI
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="my-user" password="secret";
```



### How to avoid SSL handshake errors?

When you setup a kafka cluster with a self-signed CA certificate (not official) because it's just for development, you might get an error from Conduktor:

* org.apache.kafka.common.errors.SslAuthenticationException: SSL handshake failed
* javax.net.ssl.SSLHandshakeException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target.

You need to ask (or [extract](https://dalelane.co.uk/blog/?p=4399)) the certificate of your broker and reference it from your properties:

```
security.protocol=SSL
ssl.truststore.location=/full/path/to/kafka.client.truststore.jks
ssl.truststore.password=test1234
```

See above SSL Configuration for more complete options.



### Windows and paths

If you're using Windows, you may have to use slash '/' instead of backslash '\\' to make the connection work. Here is an example when configuring a kerberos connection:

```
sasl.mechanism=GSSAPI
security.protocol=SASL_SSL
sasl.kerberos.service.name=kafka
sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required useKeyTab=true keyTab='c:/myfolder/keytab.ktf' serviceName='kafka' principal=’myid@DOMAIN.COM';
ssl.truststore.location=C:/myfolder/trust.root.jks
```

#### ERR: Illegal char <:>

If you stumbled upon this error, it means you used the "\\" character in the paths (the error shows "/" but it's wrong) :

```
Illegal char <:> at index 2: ‪C:/myfolder/key.root.jks
```

#### ERR: No Such File

If you see this error, and you are sure the path is right, try to remove the whole line and retype it yourself. You may have inserted invisible characters during copy/paste like from a Unix system (\r).

```
Failed to load SSL keystore keystore.jks‪ of type JKS
Caused by: java.nio.file.NoSuchFileException: c:/myfolder/keystore.jks‪
```

###

### Can you help us with more security troubleshooting?

Unfortunately, we cannot provide support to help you connect to your secure cluster besides what's included in the documentation. **Your Kafka administrator will have the answer to your problem**, please send them the link to this documentation page. Thank you!
