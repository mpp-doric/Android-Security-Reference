See [/api/keystore.md](/api/keystore.md) for API and usage information.

##Hardware vs Software & CTS

Some devices have hardware backed keystore storage, which was introduced in 4.3 but not manditory (Nexus devices since Nexus 4 have had this afaik). If a [CTS](http://static.googleusercontent.com/media/source.android.com/en//compatibility/android-cdd.pdf) compatable fingerprint functionality is available then

> MUST have a hardware-backed keystore implementation, and perform the fingerprint matching in a Trusted Execution Environment (TEE) or on a chip with a secure channel to the TEE.

_A [TEE](https://en.wikipedia.org/wiki/Trusted_execution_environment) (or [TPM](https://en.wikipedia.org/wiki/Trusted_Platform_Module)) can change a `KeyStore` attack from **offline** (which ould be against a software KeyStores data) to **online** (running on the device issuing commands to the hardware). This further prevents extraction of the private key and potentially throttling of access attempts (and therefore potential exploitation). See Androids [Trusty TEE](https://source.android.com/security/trusty/index.html)_

As of M-6-23 hardware backed keystore is not a requirement, but is [now manditory in N](https://youtu.be/XZzLjllizYs?t=571).

> Note that while the above TEE-related requirements are stated as STRONGLY RECOMMENDED, the
Compatibility Definition for the next API version is planned to changed these to REQIUIRED. If a
device implementation is already launched on an earlier Android version and has not implemented a
trusted operating system on the secure hardware, such a device might not be able to meet the
requirements through a system software update and thus is STRONGLY RECOMMENDED to
implement a TEE.

_Taken from [6.0 CTS](http://static.googleusercontent.com/media/source.android.com/en//compatibility/android-cdd.pdf) doc section 9.11. Keys and Credentials_

The presence of hardware backed key storage can be checked via the [`KeyChain.isBoundKeyAlgorithm`](http://developer.android.com/reference/android/security/KeyChain.html#isBoundKeyAlgorithm(java.lang.String)) method and for M-6-23+ [`KeyInfo.isInsideSecureHardware ()`](http://developer.android.com/reference/android/security/keystore/KeyInfo.html#isInsideSecureHardware()). See below

> Android also now supports hardware-backed storage for your KeyChain credentials, providing more security by making the keys unavailable for extraction. That is, once keys are in a hardware-backed key store (Secure Element, TPM, or TrustZone), they can be used for cryptographic operations but the private key material cannot be exported. Even the OS kernel cannot access this key material. While not all Android-powered devices support storage on hardware, you can check at runtime if hardware-backed storage is available by calling KeyChain.IsBoundKeyAlgorithm().

This means on a hardware backed device even if rooted the private keys cannot be extracted! They can still be used locally however (by an attacker).

> Key material may be bound to the secure hardware (e.g., Trusted Execution Environment (TEE), Secure Element (SE)) of the Android device. When this feature is enabled for a key, its key material is never exposed outside of secure hardware. If the Android OS is compromised or an attacker can read the device's internal storage, the attacker may be able to use any app's Android Keystore keys on the Android device, but not extract them from the device. This feature is enabled only if the device's secure hardware supports the particular combination of key algorithm, block modes, padding schemes, and digests with which the key is authorized to be used. To check whether the feature is enabled for a key, obtain a KeyInfo for the key and inspect the return value of KeyInfo.isInsideSecurityHardware().

and 

> If the Android OS is compromised or an attacker can read the device's internal storage, the attacker may be able to use any app's Android Keystore keys on the Android device, but not extract them from the device

The software blob can however be read on rooted devices. See [nelenkov/keystore-decryptor](https://github.com/nelenkov/keystore-decryptor).

##Links

- [Nikolay Elenkov] [Keystore redesign in Android M](https://nelenkov.blogspot.co.uk/2015/06/keystore-redesign-in-android-m.html)
