Permissions on android fall into two areas

1. How the OS handles permissions
2. How an app requests permissions

These will be outlined below

#1. Kernel Permissions

- Processes are assigned permissions based upon:
  - The owning user
  - Its PID
  - Its GIDs
  
An interesting example of inspecting a processes permissions can be seen here [Android Security: Welcome To Shell (Permissions)](http://doridori.github.io/Android-Security-welcome-to-shell/)

#2. Application Permissions

- [List of OS permissions with levels](https://github.com/android/platform_frameworks_base/blob/master/core/res/AndroidManifest.xml) 

## `android:protectionLevel`s

- Signature
  - Meaning of this depends on where the permission is defined. 
    - If in the core OS [AndroidManifest.xml](https://github.com/android/platform_frameworks_base/blob/master/core/res/AndroidManifest.xml) then permission holder would need to be signed by the same key as the OS (ROM?) 
    - If an Application defines this permission then the holder would need to be signed by the same key as the application
- System
  - An app that resides in `system/` or `system/priv/` (4.4+)  
- Normal
- Dangerous
- Privileged
- Dev
- All
