# Version 2 signing

- [v2](https://source.android.com/security/apksigning/v2.html)

# Signing with multiple certs

- Seems not vert well supported anecdotally
- [Android code signing](https://nelenkov.blogspot.co.uk/2013/04/android-code-signing.html)
  - Multiple signatures can be performed, resulting in multiple .SF and .RSA/DSA/EC files in the JAR file's META-INF/ directory.

# KeyTool

## Signing key strength

- [Guardian Project] [How to Migrate Your Android App’s Signing Key](https://guardianproject.info/2015/12/29/how-to-migrate-your-android-apps-signing-key/)
- Signiture algorithm _defaults_ depend on the `-keyalg` and `-keysize`
- `keytool` man _"The signature algorithm is derived from the algorithm of the  underlying  private  key:  If the underlying private key is of type "DSA", the default signature algorithm is "SHA1withDSA", and if the underlying private key is of type "RSA", the default signature algorithm is "MD5withRSA"."_
- `keytool` defaults change depending on JDK/Java version

```
In generating a public/private key pair, the signature algorithm (-sigalg option) is derived from the algorithm of the underlying private key:

If the underlying private key is of type "DSA", the -sigalg option defaults to "SHA1withDSA"
If the underlying private key is of type "RSA", the -sigalg option defaults to "SHA256withRSA".
If the underlying private key is of type "EC", the -sigalg option defaults to "SHA256withECDSA".
```

From http://docs.oracle.com/javase/7/docs/technotes/tools/solaris/keytool.html

Takeaway: _Always be specific!_

# Inspecting signing info

- [guardianproject/checkey](https://github.com/guardianproject/checkey)
  - Releated to interesting F-Droid [reproducable builds](https://f-droid.org/wiki/page/Deterministic,_Reproducible_Builds) project
    - [blog post](https://guardianproject.info/2015/02/11/complete-reproducible-app-distribution-achieved/)
