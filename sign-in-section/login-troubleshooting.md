# Login Troubleshooting & FAQ

## I have an internet proxy, how do I configure it?

See the [Using an Internet Proxy](internet-proxy.md) page

## Will Conduktor work offline?

Conduktor does work offline after your first login. We may require a re-login every week or so in order to refresh your token and verify your license validity. 

We don't have any "offline license" available yet. Don't hesitate to contact us if your use-case needs this.

## Oops!, something went wrong!

When you login from Conduktor Desktop and get this error in the browser, instead of having the classic login screen, this may due to several reasons.

* your browser is blocking cookies: our identity provider, Auth0, needs its cookies 🍪. You may try to open the same link \(it will be something like [https://auth.conduktor.io/u/login?state=xxx](https://auth.conduktor.io/u/login?state=xxx)\) in a private tab \(where behavior can be different and cookies allowed temporary\)
* ensure you don't have some funny extension in your browser that could alter the url for some reasons
* you hit the "back button" in your browser and tried to come back. Please relogin properly from Conduktor.
* if nothing works, try to restart Conduktor Desktop and login \(to start fresh\)

## Oh no! Authentication has failed...

When you login, you can stumbled upon this error in your browser. That means that something is either preventing Conduktor to contact our authentication server \(`https://auth.conduktor.io`\) or the other way around, something is preventing our authentication server to contact Conduktor Desktop on your computer \(outside of our control\) to finish the identification flow.

This can happen due to many reasons. Here are a few:

* Are you running Conduktor from your enterprise network?
  * You may need to configure a proxy: [https://docs.conduktor.io/sign-in-section/internet-proxy](https://docs.conduktor.io/sign-in-section/internet-proxy)
  * You may need to add a trusted certificate to Conduktor: see below
* Browser plugins can redirect http calls to httpS. The last step of our identification flow is a call to a local temporary http server \(http://localhost:5xxx\), so if something in the browser forces a redirect from http to https, the flow will never complete.
* The JVM embedded in Conduktor \(Java 13+, if you are using the classic installation process\) trusts Let's Encrypt's CA, which is the one that emits the https certificate of our authentication server https://auth.conduktor.io so nothing specific to setup here.
* Ensure you don't have an antivirus or a firewall blocking communications. You may have to add https://auth.conduktor.io to some allow-list or something.

## My organization manage its own certificates / PKIX path building failed

If your organization has its own self-signed CA and certificates, you can add trusted certificates within Conduktor from the welcome screen.

* Conduktor will create its own internal truststore when starting up
* You need to restart Conduktor after adding/removing certificates in order for them to be taken into account

![](../.gitbook/assets/screenshot-2020-05-12-at-20.26.00.png)

## I need to configure custom JVM options

Create the file `conduktor.vmoptions` in your Conduktor personal folder and add as many "-D" as you want, to set them when Conduktor starts:

* MacOS: /Users/&lt;user&gt;/Library/Application Support/conduktor/conduktor.vmoptions
* Windows: C:\Users\&lt;user&gt;\AppData\Local\conduktor\conduktor\conduktor.vmoptions
* Linux: /home/&lt;user&gt;/.config/conduktor/conduktor.vmoptions \(or XDG Config path if set\)

Example:

```text
-Djava.net.preferIPv4Stack=false
-Dhttp.proxyHost=1.2.3.4
```

## I / my company paid for a license but Conduktor tells me I'm in "free" mode!

Two situations may arise:

* You have a classic fixed license \(personal or via your company\):

Don't worry, we didn't forget your license. Conduktor's licenses are **personal** and limited to one user/machine by default. We have to be sure that you don't share your account credentials with your whole team or your whole company, that would not be fair to us 😢 \(and could be considered as a license violation\).

We don't want to lock you out from Conduktor if you do this "by accident" or if you've changed your machine, therefore we simply fallback you to the free mode of Conduktor \(all features are still available, but only with a local broker\). According to which version of Conduktor you're using, you may also see clearly **"Too many activations"** on the login screen.

Feel free to [contact us](mailto:support@conduktor.io) asap to resolve your case and remove your old machine identifications for instance.

* Your company has **floating** licenses:

Your company bought a certain number of monthly licenses \(seats\). If the current month is fully taken by your coworkers, you'll end up in the free mode for the month and you'll see **"No seats available"** on your login screen. Contact your technical administrator, your company may have to update its subscription to add more seats.

## My issue is not addressed here

Please [send us an email](mailto:support@conduktor.io?subject=Login%20Troubleshooting?body=Please%20include%20as%20much%20information%20as%20possible,%20as%20well%20as%20screenshots,%20or%20even%20better,%20videos)

