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

## Declaring custom permissions

See [\<permission\>](https://developer.android.com/guide/topics/manifest/permission-element.html)

## android:protectionLevels

[R.attr list](https://developer.android.com/reference/android/R.attr.html#protectionLevel)

###3rd party app definable:

- Normal
  - The system automatically grants this type of permission to a requesting application at installation
- Dangerous
  - This type of permission introduces potential risk, the system may not automatically grant it to the requesting application 
- Signature
  - Meaning of this depends on where the permission is defined. 
    - If in the core OS [AndroidManifest.xml](https://github.com/android/platform_frameworks_base/blob/master/core/res/AndroidManifest.xml) then permission holder would need to be signed by the same key as the OS (ROM?) 
    - If an Application defines this permission then the holder would need to be signed by the same key as the application
  - Permission will be auto granted if sig check passes 
- signatureOrSystem 
  - A permission that the system grants only to applications that are in the Android system image or that are signed with the same certificate as the application that declared the permission.  
  - `System` has same requirements as below 
  
###Other:

- System
  - An app that resides in `system/app` (<4.4) or `system/priv-app/` (4.4+) [link](http://stackoverflow.com/a/20104400/236743) 
  - [Now deprecated](https://developer.android.com/reference/android/content/pm/PermissionInfo.html#PROTECTION_FLAG_SYSTEM) 
  - [Old synonym for "privileged".](https://developer.android.com/reference/android/R.attr.html#protectionLevel)
- Privileged
  - An app that resides in `system/priv-app/` (4.4+)  
  - Superceeds `system`
- Preinstalled 
  - An app that resides in `system/app` (4.4+)? [link](http://stackoverflow.com/questions/33481730/difference-between-preinstalled-and-privileged-protection-level)
- [Various](https://developer.android.com/reference/android/R.attr.html#protectionLevel)
