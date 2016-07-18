#Https CAs

> To provide a more consistent and more secure experience across the Android ecosystem, beginning with Android Nougat, compatible devices trust only the standardized system CAs maintained in AOSP. 

From [android-developers.blogspot] [Changes to Trusted Certificate Authorities in Android Nougat](http://android-developers.blogspot.co.uk/2016/07/changes-to-trusted-certificate.html)

###Dev/DEBUG settings

Starting in N can add custom trust anchors via xml which are used only for debug builds. Removes the vuln of accidentially leaving debug code in production. [IO link](https://youtu.be/XZzLjllizYs?t=1405)

###Chain info tools

- https://langui.sh/2009/03/14/checking-a-remote-certificate-chain-with-openssl/

#Enforcing Https

From [Protecting against unintentional regressions to cleartext traffic in your Android apps](https://security.googleblog.com/2016/04/protecting-against-unintentional.html)

- `android:usesCleartextTraffic=”false”` in manifest for M+ (6.0+). [Docs](https://developer.android.com/guide/topics/manifest/application-element.html#usesCleartextTraffic)
- `StrictMode.VmPolicy.Builder.detectCleartextNetwork()` in development


