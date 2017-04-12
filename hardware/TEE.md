# General Info

- See [Trusty TEE](https://source.android.com/security/trusty/index.html)
- See [Mobile Platform Security: Trusted Execution Environments](http://asokan.org/asokan/Padova2014/tutorial-mobileplatsec.pdf)

From 'Android Security Internals' the Nexus 4 was TrustZone enabled, with QSEE implemented on top of that. Most likely uses a single master key-encryption key (KEK) which protects all user-generated keys but implementations may use KEK per key and / or hardware fused KEKs.

# Players

- [ARM TrustZone](https://www.arm.com/products/security-on-arm/trustzone)
  - TrustZone is hardware-based security built into SoCs by semiconductor chip designers who want to provide secure end points and a device root of trust. 
  - TrustZone technology provides a foundation for system-wide security and the creation of a trusted platform. Any part of the system can be designed to be part of the secure world, including debug, peripherals, interrupts and memory.
  - TrustZone technology within Cortex-A based application processors is commonly used to run trusted boot and a trusted OS to create a Trusted Execution Environment (TEE)
    - [Cortex-A](https://www.arm.com/products/processors/cortex-a) processors power many mobile devices. For example, the Pixel phone uses a [Kryo](https://en.wikipedia.org/wiki/Kryo_(microarchitecture)) microarchitecture/CPU (as part of the Snapdragon 821 processor SoC) using the ARMv8-A Instruction Set. 
- Qualcomm's Secure Execution Environment (QSEE)
  - Found on Snapdragon SoCs
  - [Exploring Qualcomm's Secure Execution Environment](http://bits-please.blogspot.co.uk/2016/04/exploring-qualcomms-secure-execution.html)

# Vulns

- [Extracting Qualcomm's KeyMaster Keys – Breaking Android Full Disk Encryption](https://news.ycombinator.com/item?id=12007923) _30/6/2016_
  - Shows how the KEK can be lifted from Android devices and claims not fixable i.e. a hardware issue. Talks about how this means FDE (full disk encryption) is not bound to the device hardware, which is bad as it could easily be brute-forced off-device. 
  - Likely to have implications to `KeyStore` use
  
# Usage

- Verifying kernel integrity (TIMA)
- Using the Hardware Credential Storage (used by "keystore", "dm-verity")
- Secure Element Emulation for Mobile Payments
- Implementing and managing Secure Boot
- DRM (e.g. PlayReady)
- Accessing platform hardware features (e.g. hardware entropy)

# Glossary 

- `Secure World` - code that runs in a "Trusted" secure enviroment. In contrast to `Normal World`.

# Links

- Bits Please Series
  - [1: Getting arbitrary code execution in TrustZone's kernel from any context](http://bits-please.blogspot.co.uk/2015/03/getting-arbitrary-code-execution-in.html)
    - Has intro to TrustZone
  - [2: Exploring Qualcomm's TrustZone implementation](http://bits-please.blogspot.co.uk/2015/08/exploring-qualcomms-trustzone.html)
  - Talks about the SMC (`Secure Monitor Calls`) driver opcode calls for `normal-world` to call into `secure-world`
  
