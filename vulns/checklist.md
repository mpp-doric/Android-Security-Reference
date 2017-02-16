The below can be used as a checklist for your application.

#Vuln Format

Follows format of:

**Vuln:`<tld vuln name>`**

- Vuln:`<vuln name>`
  - `[info:<description>]` _optional_
  - `<mitigations>`

#Vuln: Data At Rest

- Vuln: Filesystem Data
	- Stored on disk using a hardware / software `KeyStore` backed AES key. Keystore access is mitigated via OS level permissions.
	- `Keystore` is encrypted with device lockscreen method (PIN|PATTERN|PW|FINGER?)
	- Data retreived with integrity checks
	- Should be stored using a `PRIVATE` mode

#Vuln: Data In Use

- Vuln: Memory Data
	- Only kept in memory for as short a time as possible
	- When finished with data is zeroed out / overwritten where possible instead of just leaving for GC
	- Debugger Detection
		- Native Debugger Defenses [p54](https://regmedia.co.uk/2016/09/02/hacking_soft_tokens_-_bernhard_mueller.pdf)
		- JDWP Debugger Detection [p56](https://regmedia.co.uk/2016/09/02/hacking_soft_tokens_-_bernhard_mueller.pdf)

#Vuln: Data In Transit

- Vuln: Network
	- Vuln: SSL downgrade to http
		- Server should not serve over http 
		- Okhttp `.followRedirects(false)`
		- `android:usesCleartextTraffic=”false”`
		- `StrictMode.VmPolicy.Builder.detectCleartextNetwork()` in development
	- Vuln: older than TLS 1.2
		- Enforce TLS 1.2		
	- Vuln: low security cipher suite 
		- Enforce strong cipher suite i.e. TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
    - Vuln: MITM and endpoint integrity
		- SSL cert pinning (Can now do via okhttp)
    - Vuln: Sensitive data held is app level / OS level networking stack 
		- Disable network caching at http stack level
    - Vuln: SSL bypassed 
		- Pin transmission is optionally hidden inside the SIGMA session

#Vuln: Data Entry / Display

- Vuln: Screen Capture
    - set FLAG_SECURE otherwise screen can be read / captures with stock apis
    - ensure sensitive information entered by secure IMEs only. [PSA: FLAG_SECURE Window Leaks](https://commonsware.com/blog/2016/06/06/psa-flag-secure-window-leaks.html)
    - check for open file handle frame buffers. [Some OEMs leave these open](http://stackoverflow.com/questions/11271436/how-to-capture-a-screenshot-of-other-android-applications-from-an-app-without-ro/11273364#11273364)
    - `screencap` is for `system` users only so can ignore  
- Vuln: Screen Caches 
	- Disable default OS view state saving via `android:saveEnabled` | `activity android:noHistory=true`
- Vuln: Tap Jacking
	- `View.setFilterTouchesWhenObscured(true)`
	- `onFilterTouchEventForSecurity(MotionEvent)`
- Vuln: Accessability Layer
  - Info: Can listen to all keypresses, and have access to view hierarchy via Accessability API 
  - Check if any custom [AccessabilityService](http://developer.android.com/reference/android/accessibilityservice/AccessibilityService.html) active as has access to all View objects and

#Vuln: Data Leakage

- Vuln: Log output
    - Compile time obfuscator tools strip out
- Vuln: Exception output
    - Global exception handler so not output to log
- Vuln: Intent Extras
    - Dont pass sensitive data in intent extras and can be read / spoofed
- Vuln: Persisted Background State
    - Detail: Android Bundle - Activity / Fragment Saved State `Bundle` - OS may keep in memory for a while and also ambigous if written to disk when process killed
    - dont write sensitive stuff in Bundles. See `Vuln: Screen Caches` above for more.
- Vuln: Application Backups
    - `allowBackups=false` in manifest to disable auto backups 

#Vuln: Rooted Device

- Mitigation: Root detection
	- looks for common root mgmt apps
	- looks for apps commonly found on rooted devices
	- looks for `su` binary by common names
	- looks for busybox binary
	- .props file check (`ro.secure`)
	- check perms on `/system`
	- check ROM keys
	- 3rd party tools / libs

#Vuln: Broken Crypto

- Vuln: Broken OS Crypto
- Vuln: Compromised OS Crypto
- Vuln: Broken Crypto Implementation
  - Should use force updated CryptoProvider through GPlayServices 

#Vuln: Static Analysis 

Static and Dynamic Analysis can be used for a multitude of attacks. The main vulns are:

- Vuln: API Discovery
	- Vuln: API Discovery (Dynamic)
		- See `Dynamic MITA` below
	- Vuln: API Discovery (Static)
		- Obfuscation tooling
		- Check if running on emulator 
		- Move to C as harder to decompile etc

#Vuln: Dynamic Analysis

Could be on users device i.e. inspecting memory or attackers device to understand app.

- Vuln: Dynamic MITA (Man In The App) 
	- Vuln: Debuggable 
		- Device. `.props` file check (`ro.debuggable`) 
		- App. Check `debuggable` not set
  - Vuln: Mem dumps
    - Mitigate _debuggable_ as above
    - Ensure device does not dump .hprof on [`kill -10`](http://stackoverflow.com/questions/5640182/why-cant-i-get-an-hprof-dump-from-certain-devices)
  - Mem hook detection (ext tooling)
  - open shell detection

#Vuln: Code Reuse

- Vuln: App Repackaging (Malware)
	- Checksumming 

#Vuln: Code execution

- Vuln: Exported Components
        - Ensure components are non-exported unless required and validate all inputs 
- Vuln: Input Injection
        - Validate UI input
- Vuln: Javascript 
        - Follow latest practise for safe exposing of Java APIs to Javascript 
	
#Vuln: OS bugs

Below is a list of patches that need to be applied as to not expose officially patched Vulns

- SecureRandom ([fix](http://android-developers.blogspot.co.uk/2013/08/some-securerandom-thoughts.html))
- PlayServices version ([SSL fix](http://developer.android.com/training/articles/security-gms-provider.html))

#Vuln: Custom Permissions

- Pre 5 check for pre-held permission declarations [The Custom Permission Problem](https://github.com/commonsguy/cwac-security/blob/master/PERMS.md)

#Vuln: Dependencies

- Verify integrity TOFU style
  - [Gradle Witness](https://github.com/WhisperSystems/gradle-witness)
