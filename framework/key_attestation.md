# Gives

- Passed verified boot
- has recent security patch
- key is protected by TEE or StrongBox
- Key requires user auth in X seconds
- Firmware hash (compatible P+ devices)

# Hardware or software backed

- All 7 implementations are software backed
- 8 is optional

# OS version changes

- **N-7-24**
  - [Key Attestation](https://developer.android.com/training/articles/security-key-attestation.html#certificate_schema)
    - Can prove to 3rd partys that a hardware keystore exists with certain keys by signing a representation using a factory supplied key 
    - KeyStore now returns a chain for a key alias which can be used to verify that the device has passed CTS testing
    - All devices shipping with 7 had software attestation only
  - Hardware KeyStore manditory [IO link](https://youtu.be/XZzLjllizYs?t=571) 
  - "When the device implementation supports a secure lock screen it MUST back up the keystore implementation with secure hardware" [ACD](http://source.android.com/compatibility/7.0/android-7.0-cdd.html#9_11_keys_and_credentials)
  - "Note that if a device implementation is already launched on an earlier Android version, and does not have a fingerprint scanner, such a device is exempted from the requirement to have a hardware-backed keystore." also from keys and creds
- **O-8-26**
  - All O+ devices have attestation API
  - If shipped with 0 the attestation API MUST be hardware backed ([CDD](https://source.android.com/compatibility/8.0/android-8.0-cdd#9_11_keys_and_credentials))
