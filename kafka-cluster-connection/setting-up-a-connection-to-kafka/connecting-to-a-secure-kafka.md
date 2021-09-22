---
description: >-
  Connecting to a secure Kafka cluster may be difficult, but hopefully, this
  page will help you. Conduktor inherits the permissions of the user that
  connects to Kafka.
---

# Connecting to a Secure Kafka

## YouTube video walkthrough

{% embed url="https://youtu.be/\_NQmMUQL07Y" %}

## General Idea

Conduktor leverages the default Apache Kafka Java Clients, and therefore we use the same [configuration properties](https://kafka.apache.org/documentation/#consumerconfigs). **If you are trying to connect to a secure Kafka cluster using Conduktor, please first try to use the CLI.** If you don't know how, please contact your administrator. 

**Example:**

```bash
kafka-console-consumer \
    --topic my-topic \
    --bootstrap-server SASL_SSL://kafka-url:9093 \
    --consumer.config config.properties
```

Your `config.properties` file may contain something like this:

```text
security.protocol=SSL
ssl.truststore.location=/var/private/ssl/client.truststore.jks
ssl.truststore.password=test1234
other.configs=values...
```

What Conduktor needs to connect to a secure Kafka cluster is all the values from your `config.properties` file.

{% hint style="info" %}
In case you don't know what should be the values in the `config.properties` file, please contact your Kafka administrator. 

**Note:** these are the same properties you would use in your Kafka Java clients or applications. 
{% endhint %}

## SSL Configuration

If client authentication is not required by the broker, the following is a minimal configuration example:

```text
security.protocol=SSL
ssl.truststore.location=/full/path/to/kafka.client.truststore.jks
ssl.truststore.password=test1234
```

If client authentication is required, then a keystore must be created for each client, and the brokers’ truststores must trust the certificate in the client’s keystore. Please ask your Kafka administrator for help on generating client keys. Here is a configuration example:

```text
security.protocol=SSL
ssl.truststore.location=/full/path/to/kafka.client.truststore.jks
ssl.truststore.password=test1234
ssl.keystore.location=/full/path/to/kafka.client.keystore.jks
ssl.keystore.password=test1234
ssl.key.password=test1234
```

Other configuration settings that may also be needed depending on our requirements and the broker configuration:

1. ssl.provider \(Optional\). The name of the security provider used for SSL connections. Default value is the default security provider of the JVM.
2. ssl.cipher.suites \(Optional\). A cipher suite is a named combination of authentication, encryption, MAC and key exchange algorithm used to negotiate the security settings for a network connection using TLS or SSL network protocol.
3. ssl.enabled.protocols=TLSv1.2,TLSv1.1,TLSv1. It should list at least one of the protocols configured on the broker side
4. ssl.truststore.type=JKS
5. ssl.keystore.type=JKS

{% hint style="warning" %}
Please make sure to put the **full paths** to your SSL certificates in your properties file  
Relative paths may not work for Conduktor. 
{% endhint %}

## SASL Configuration

Multiple SASL configurations can be done for Apache Kafka and Conduktor supports them all. In this documentation we will just cover Kerberos, but you should get a general sense of how things work. 

Here's a minimal configuration for SASL\_PLAINTEXT:

```text
security.protocol=SASL_PLAINTEXT
sasl.mechanism=GSSAPI
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="my-user" password="secret";
```

### SCRAM configuration

Use this configuration in Conduktor:

```text
security.protocol=SASL_SSL
sasl.mechanism=SCRAM-SHA-512
sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required username="yyy" password="xxx";
```

### 🚨 About JAAS files

If you see a JAAS file being passed as a Java option to your Kafka clients using

```text
-Djava.security.auth.login.config=/etc/kafka/kafka_client_jaas.conf
```

then you must you the `sasl.jaas.config` property as outlined above in Conduktor.

**Example:** the following JAAS file:

```text
KafkaClient {
  org.apache.kafka.common.security.plain.PlainLoginModule required
  username="alice"
  password="alice-secret";
};
```

Would be converted to the following `sasl.jaas.config` property:

```text
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required  username="alice" password="alice-secret";
```

{% hint style="warning" %}
Please make sure to put the **full paths** to your SASL key files in your properties file  
Relative paths may not work for Conduktor. 
{% endhint %}

### Example using Kerberos

Another example using Kerberos and a keytab:

* a JAAS file \(would need -Djava.security.auth.login.config=/path/to/jaas.conf\)

```text
KafkaClient {
  com.sun.security.auth.module.Krb5LoginModule required
  useKeyTab=true
  keyTab="/etc/security/keytabs/alice.keytab"
  principal="alice@EXAMPLE.COM";
};
```

* The same, but using `sasl.jaas.config`:

```text
sasl.kerberos.service.name=kafka
sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required useKeyTab=true keyTab="/etc/security/keytabs/alice.keytab" principal="alice@EXAMPLE.COM";
```

## Windows and paths

If you're using Windows, you may have to use slash '/' instead of backslash '\' to make the connection work. Here is an example when configuring a kerberos connection:

```text
sasl.mechanism=GSSAPI
security.protocol=SASL_SSL
sasl.kerberos.service.name=kafka
sasl.jaas.config=com.sun.security.auth.module.Krb5LoginModule required useKeyTab=true keyTab='c:/myfolder/keytab.ktf' serviceName='kafka' principal=’myid@DOMAIN.COM';
ssl.truststore.location=C:/myfolder/trust.root.jks
```

#### ERR: Illegal char &lt;:&gt;

If you stumbled upon this error, it means you used the "\" character in the paths \(the error shows "/" but it's wrong\) :

```text
Illegal char <:> at index 2: ‪C:/myfolder/key.root.jks
```

#### ERR: No Such File

If you see this error, and you are sure the path is right, try to remove the whole line and retype it yourself. You may have inserted invisible characters during copy/paste like from a Unix system \(\r\).

```text
Failed to load SSL keystore keystore.jks‪ of type JKS
Caused by: java.nio.file.NoSuchFileException: c:/myfolder/keystore.jks‪
```

## Can you help us with security troubleshooting?

Unfortunately, we cannot provide support to help you connect to your secure cluster besides what's included in the documentation. **Your Kafka administrator will have the answer to your problem**, please send them the link to this documentation page. Thank you!

