# Login Troubleshooting & FAQ

## I have an internet proxy, how do I configure it?

See the [Using an Internet Proxy](internet-proxy.md) page

## Will Conduktor work offline?

Conduktor does work offline after your first login. We require a re-login every week or so in order to refresh your token and verify your license validity. 

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

## 

## My issue is not addressed here

Please [send us an email](mailto:support@conduktor.io?subject=Login%20Troubleshooting?body=Please%20include%20as%20much%20information%20as%20possible,%20as%20well%20as%20screenshots,%20or%20even%20better,%20videos)

