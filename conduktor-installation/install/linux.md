# Linux

## General Steps 

Conduktor supports Linux, but Linux is a very wide ecosystem, and it is hard for us to support all Linux distributions. We're doing our best to improve and package Conduktor for Linux, don't hesitate to contact us in case of bugs. 

We have prepared three distributions for Conduktor

## Debian

You can use a `.deb` file to install Conduktor on your Debian. This is the distribution you would be looking at if you're using **Ubuntu**.

In this example we are using Ubuntu and therefore the`.deb` file, example: `Conduktor-2.0.13.deb` 

You can easily install Conduktor using the following command

```text
sudo apt install ./Conduktor-2.0.13.deb
```

You may be prompted to entire your admin credentials so that Conduktor can be installed

![](../../.gitbook/assets/image%20%2827%29.png)

Conduktor is then installed in

```text
$ ll /opt/conduktor/
total 20
drwxr-xr-x 5 root root 4096 Apr  9 13:11 .
/drwxr-xr-x 3 root root 4096 Apr  9 13:11 ../
drwxr-xr-x 2 root root 4096 Apr  9 13:11 bin/
drwxr-xr-x 4 root root 4096 Apr  9 13:11 lib/
drwxr-xr-x 3 root root 4096 Apr  9 13:11 share/
```

**To start Conduktor, you should be able to find it in your Ubuntu applications**

![](../../.gitbook/assets/image%20%2823%29.png)

Alternatively, you can run:

```text
/opt/conduktor/bin/Conduktor
```

## RPM

You can use a `.rpm` file to install Conduktor on your RPM based system. This includes Red Hat Linux, CentOS, Fedora, OpenSUSE.

## Zip distro 

This is not the recommended way to install Conduktor, but if you have a JRE 11+ installed on your system, you can try to use the `.zip` distribution of Conduktor. Overall, we cannot guarantee this method will work but we offer it to you as an alternative to the `.deb` and `.rpm` bundles.

## **Signing in**

The next step is to sign in to Conduktor. See this section here



## Installation Issues

### Ubuntu Focal 20: missing libffi6 when installing .deb

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

### **I have a HDPI monitor and Conduktor doesn't scale on Linux**

We are unfortunately limited by the underneath technologies Java and JavaFX. It's possible to add JVM options to Conduktor's JVM configuration to alter the behaviour of the JVM:

```text
/home/user/.config/conduktor/v1/app.properties
```

Unfortunately, despite everything we can find on the internet, and the community help, nothing work:

```text
-Dsun.java2d.dpiaware=true
-Dhidpi=true
-Dsun.java2d.xrender=true
-Dide.ui.scale=2.0
-Dsun.java2d.uiScale.enabled=true
-Dsun.java2d.uiScale=2
```

Nor `export GDK_SCALE=2`.

{% hint style="info" %}
**Bounty:** If you know how to make this happen, or have some pointers, [feel free to contact us](https://www.conduktor.io/contact/). We'll offer you a Conduktor professional license. 
{% endhint %}

### I can't find my issue here

Please [contact us](https://www.conduktor.io/contact)

