---
description: A list of common question our community asks
---

# Frequently Asked Questions

## Convert a certificate to a Java Trust Store

In the Java world \(hence in Conduktor\), we work with the "Java KeyStore .jks" format instead of the common PEM format \(.pem, .crt, ..\). Hopefully, here is a command line to convert a certificate `mycompany.crt` to a useful keystore `mycompany.jks`  \(with password "changeit"\) to be used as truststore:

```text
keytool -import -v -trustcacerts -alias mycompany -file mycompany.crt -keystore mycompany.jks -keypass changeit -storepass changeit
```

## Setup the Keystore in Conduktor

Conduktor supports explicit truststore/keystore for some of our HTTPS integrations: Kafka Connect, ksqlDB. Kafka & Schema Registry also support truststore/keystore through their additional properties. 

In case we're missing an integration, it can be useful to setup it globally on Conduktor itself \(it does not mean it is inherited by the specific connection that already manage a specific keystore configuration\):

```text
-Djavax.net.ssl.keyStore=/home/xxx/my.client.keystore.jks
-Djavax.net.ssl.keyStorePassword=<password>
```

See [https://docs.conduktor.io/misc/configuring-conduktor\#custom-environment-variables](https://docs.conduktor.io/misc/configuring-conduktor#custom-environment-variables) to see where.

## I'm using IPv6 infrastructure

By default, due to Java "habits", and to avoid complicated issues and troubleshoots \(such as "Conduktor can't connect to.."\), Conduktor automatically set `-Djava.net.preferIPv4Stack=true` to automatically use the IPv4 stack \(which is still mostly used\) when starting up.

If you have no problem with IPv6, if your infrastructure is up to date and servers bound to IPv6 addresses, then you may run into troubles, and you'll need to disable this option.

To do this, create the file `conduktor.vmoptions` in your Conduktor personal folder and disable the option \(restart Conduktor to be taken into account\):

* MacOS: /Users/&lt;user&gt;/Library/Application Support/conduktor/conduktor.vmoptions
* Windows: C:\Users\&lt;user&gt;\AppData\Local\conduktor\conduktor\conduktor.vmoptions
* Linux: /home/&lt;user&gt;/.config/conduktor/conduktor.vmoptions \(or XDG Config path if set\)

```text
# in conduktor.vmoptions, supports only -D* options

-Djava.net.preferIPv4Stack=false
```

Sometimes, it seems it is not enough. Another solution is to remove the original flag in the original configuration: be aware that this modification will be LOST each time you will upgrade Conduktor.

Remove `-Djava.net.preferIPv4Stack=true` in the source configuration file of Conduktor:

* MacOS: /Applications/Conduktor.app/Contents/app/Conduktor.cfg
* Linux: /opt/conduktor/lib/app/Conduktor.cfg
* Windows:

## Ubuntu Focal 20: missing libffi6 when installing .deb

For Linux, Conduktor is packaged with libffi6. This library had seen its version bump to libffi7 in Ubuntu 20 Focal so you'll end up with this error:

```text
$ sudo dpkg -i Conduktor-2.1.1.deb 
...
dpkg: dependency problems prevent configuration of conduktor:
 conduktor depends on libffi6; however:
  Package libffi6 is not installed.
```

It's possible to install the previous version using coming from Ubuntu 19.10 \(Eoan Ermine\): download it here [http://mirrors.kernel.org/ubuntu/pool/main/libf/libffi/libffi6\_3.2.1-8\_amd64.deb](http://mirrors.kernel.org/ubuntu/pool/main/libf/libffi/libffi6_3.2.1-8_amd64.deb)

```text
$ curl -LO http://archive.ubuntu.com/ubuntu/pool/main/libf/libffi/libffi6_3.2.1-9_amd64.deb
$ sudo dpkg -i libffi6_3.2.1-9_amd64.deb 
$ sudo dpkg -i Conduktor-2.1.1.deb 
... OK!
```

## **I have a HDPI monitor and Conduktor doesn't scale on Linux**

To scale Conduktor, you can use GDK\_SCALE \(with a plain integer\), such as:

```text
$ GDK_SCALE=2 /opt/conduktor/bin/Conduktor
```

Or you can also modify the shortcut directly

```text
$ sudo vim /usr/share/applications/conduktor-Conduktor.desktop
...
Exec=env GDK_SCALE=2 /opt/conduktor/bin/Conduktor
```

If it does not work, we are unfortunately limited by the underneath technologies Java and JavaFX. Although it's possible to add JVM options to Conduktor's JVM configuration to alter the behaviour of the JVM, but we never found a working solution.

The following flags does not work:

```text
# Config in: /home/user/.config/conduktor/v1/app.properties

-Dsun.java2d.dpiaware=true
-Dhidpi=true
-Dsun.java2d.xrender=true
-Dide.ui.scale=2.0
-Dsun.java2d.uiScale.enabled=true
-Dsun.java2d.uiScale=2.
```

### 

