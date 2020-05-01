---
description: A list of common question our community asks
---

# FAQ

**I have a HDPI monitor and Conduktor doesn't scale on Linux**

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

If you know how to make this happen, or have some pointers, [feel free to contact us](https://www.conduktor.io/contact/).



