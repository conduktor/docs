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

### I can't find my issue here

Please [contact us](https://www.conduktor.io/contact)

