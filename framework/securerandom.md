# SecureRandom

- [CDD](https://source.android.com/compatibility/7.0/android-7.0-cdd.html) doc contains no info on `SecureRandom`
- Src can be found at `libcore/luni/src/main/java/java/security/SecureRandom.java`

## What happens when calling `SecureRandom`?

### Android 6

- new SecureRandom()  
  - If no `Services.getSecureRandomService()` then uses SHA1PRNG (provided by [`AndroidOpenSSL` and `Crypto`](http://stackoverflow.com/a/40913256/236743))
  - otherwise returns first `Engine.door.getService` service/provider that supports `SecureRandom`
  
### Android 7

SecureRandom has changed in 7 (the `Crypto` provider was removed, which removed one of two SHA1PRNG implementations from stock android) and the javadoc has been [updated](https://developer.android.com/reference/java/security/SecureRandom.html) with test info

## /urandom

- [Android empties the entropy pool, resulting in blocking, user perceived lag/poor performance](https://code.google.com/p/android/issues/detail?id=42265)
  - States since 2.3 Android has used `dev/urandom`
- [Myths about /dev/urandom](http://www.2uo.de/myths-about-urandom/)

## Links

- [android-developers.blogspot.co.uk] [Using Cryptography to Store Credentials Safely](http://android-developers.blogspot.co.uk/2013/02/using-cryptography-to-store-credentials.html)
- [android-developers.blogspot.co.uk] [Security "Crypto" provider deprecated in Android N](http://android-developers.blogspot.co.uk/2016/06/security-crypto-provider-deprecated-in.html)
- [android-developers.blogspot.co.uk] [Some SecureRandom Thoughts](http://android-developers.blogspot.co.uk/2013/08/some-securerandom-thoughts.html)
