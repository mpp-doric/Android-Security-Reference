# Version 2 signing

- [v2](https://source.android.com/security/apksigning/v2.html)

# Signing with multiple certs

- Seems not vert well supported anecdotally
- [Android code signing](https://nelenkov.blogspot.co.uk/2013/04/android-code-signing.html)
  - Multiple signatures can be performed, resulting in multiple .SF and .RSA/DSA/EC files in the JAR file's META-INF/ directory.

# Signing key strength

- [How to Migrate Your Android App’s Signing Key](https://guardianproject.info/2015/12/29/how-to-migrate-your-android-apps-signing-key/)

# Inspecting signing info

- [guardianproject/checkey](https://github.com/guardianproject/checkey)
  - Releated to interesting F-Droid [reproducable builds](https://f-droid.org/wiki/page/Deterministic,_Reproducible_Builds) project
