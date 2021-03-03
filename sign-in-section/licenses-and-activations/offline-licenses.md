---
description: 'For companies without internet access, offline license is the way to go!'
---

# Offline Licenses

Conduktor relies on Auth0 to manage user authentication securely and also check our licenses. Therefore, it is not possible to use Conduktor if you don't have an online access. This is why we developed an offline mode.

## Process

To work in offline mode, there is a few steps to be followed by the user and the manager.

### User: Enable offline Login

To avoid using auth0, Conduktor can identify you using a "offline token" instead. Doing such, no contact to the outside world is necessary, and you can work offline, with your clusters still accessible on your network.

To get this "offline token", you need to enable the Offline mode into Conduktor by going into the Network options, on the welcome screen:

![Go to Network &amp;gt; Options, enable offline Login](../../.gitbook/assets/screenshot-2021-03-03-at-22.18.12.png)

By doing so, the "LOGIN / SIGNUP" button now reads "OFFLINE LOGIN".

### User: Click on Offline Login

This is open a dialog where you have to enter your email address, then click on "Generate Code".

This will generate a code **specific for your account and your machine**, that you need to send to your Conduktor manager. Every code generated will be different.

![](../../.gitbook/assets/screenshot-2021-03-03-at-22.21.34.png)

### Manager: convert your code to an "offline token"

Using our portal \(through an online access\), the manager of your team / company will be able to create an "offline token" for Conduktor, from a code.

A token is specific PER USER. All users will have a different token.

### User: insert its own "offline token" into Conduktor

In the Step 2, you just paste it, then click on Login:

![](../../.gitbook/assets/screenshot-2021-03-03-at-22.39.40.png)

Et voilà! You are "offline logged in"! You can show the "Offline mode" enabled on the bottom left.

![](../../.gitbook/assets/screenshot-2021-03-03-at-22.40.46.png)

## A few things

### Offline means nothing goes out

Because you are offline, some options are now disabled and hidden \(like Analytics & Reports: they have nowhere to go!\).

### By Machine 

A code is generated for a specific machine \(where Conduktor is installed\). A token generated is also specific for this same machine \(where the original code was generated\).

You cannot use a token on a different machine, you will need to generate a new token for the user \(hence a new license\).

### Renewal every year

Subscriptions are typically renewed every year.

Therefore, the generated tokens will be declared **INVALID** by Conduktor on the subscription anniversary date \(there is _one day_ of slack to provide a "smooth" migration\). All tokens will need to be generated again for all users under this mode.





