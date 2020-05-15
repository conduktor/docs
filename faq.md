---
description: A list of common question our community asks
---

# FAQ

## Ubuntu Focal 20: missing libffi6 when installing .deb

For Linux, Conduktor is packaged with libffi6. This library had seen its version bump to libffi7 in Ubuntu 20 Focal so you'll end up with this error:

```text
$ sudo dpkg -i Conduktor-2.1.1.deb 
...
dpkg: dependency problems prevent configuration of conduktor:
 conduktor depends on libffi6; however:
  Package libffi6 is not installed.
```

It's possible to install the previous version using coming from Ubuntu 19.10 \(Eoan Ermine\): download it here [https://ubuntu.pkgs.org/19.10/ubuntu-main-amd64/libffi6\_3.2.1-9\_amd64.deb.html](https://ubuntu.pkgs.org/19.10/ubuntu-main-amd64/libffi6_3.2.1-9_amd64.deb.html)

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

