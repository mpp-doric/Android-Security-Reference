# General Info

- See [Trusty TEE](https://source.android.com/security/trusty/index.html)
- See [Mobile Platform Security: Trusted Execution Environments](http://asokan.org/asokan/Padova2014/tutorial-mobileplatsec.pdf)

From 'Android Security Internals' the Nexus 4 was TrustZone enabled, with QSEE implemented on top of that. Most likely uses a single master key-encryption key (KEK) which protects all user-generated keys but implementations may use KEK per key and / or hardware fused KEKs.

# Players

- [ARM TrustZone](https://www.arm.com/products/security-on-arm/trustzone)
  - TrustZone is hardware-based security built into SoCs by semiconductor chip designers who want to provide secure end points and a device root of trust. 
  - TrustZone technology provides a foundation for system-wide security and the creation of a trusted platform. Any part of the system can be designed to be part of the secure world, including debug, peripherals, interrupts and memory.
  - TrustZone technology within Cortex-A based application processors is commonly used to run trusted boot and a trusted OS to create a Trusted Execution Environment (TEE)
    - [Cortex-A](https://www.arm.com/products/processors/cortex-a) processors power many mobile devices. For example, the Pixel phone uses a [Kryo](https://en.wikipedia.org/wiki/Kryo_(microarchitecture)) CPU using the ARMv8-A Instruction Set. 
- Qualcomm's Secure Execution Environment (QSEE)
  - Found on Snapdragon SoCs
  - [Exploring Qualcomm's Secure Execution Environment](http://bits-please.blogspot.co.uk/2016/04/exploring-qualcomms-secure-execution.html)

# Vulns

- [Extracting Qualcomm's KeyMaster Keys – Breaking Android Full Disk Encryption](https://news.ycombinator.com/item?id=12007923) _30/6/2016_
  - Shows how the KEK can be lifted from Android devices and claims not fixable i.e. a hardware issue. Talks about how this means FDE (full disk encryption) is not bound to the device hardware, which is bad as it could easily be brute-forced off-device. 
  - Likely to have implications to `KeyStore` use

