#Application Permissions

## Declaring custom permissions

See [\<permission\>](https://developer.android.com/guide/topics/manifest/permission-element.html)

## android:protectionLevels

See comprehensive official list [R.attr list](https://developer.android.com/reference/android/R.attr.html#protectionLevel). Notes below:

###Apk definable:

- `Normal`
  - The system automatically grants this type of permission to a requesting application at installation
- `Dangerous`
  - This type of permission introduces potential risk, the system may not automatically grant it to the requesting application 
- `Signature`
  - Meaning of this depends on where the permission is defined. 
    - If in the core OS [/system/framework/framework-res.apk/AndroidManifest.xml](https://github.com/android/platform_frameworks_base/blob/master/core/res/AndroidManifest.xml) then the permission holder would need to be signed by the same key (which is the _`platform`_ key used in the OS signing process)
    - If an Application defines this permission then the holder would need to be signed by the same key as the application
  - Permission will be auto granted if sig check passes 
- `signatureOrSystem` 
  - See `signiture` and `system`
  
###Other:

- `System`
  - An app that resides in `system/app` (<4.4) or `system/priv-app/` (4.4+) [link](http://stackoverflow.com/a/20104400/236743) 
  - [Now deprecated](https://developer.android.com/reference/android/content/pm/PermissionInfo.html#PROTECTION_FLAG_SYSTEM) 
  - [Old synonym for "privileged".](https://developer.android.com/reference/android/R.attr.html#protectionLevel)
- `Privileged`
  - An app that resides in `system/priv-app/` (4.4+)  
  - Superceeds `system`
- `Preinstalled` 
  - An app that resides in `system/app` (4.4+)? [link](http://stackoverflow.com/questions/33481730/difference-between-preinstalled-and-privileged-protection-level)
- [Various](https://developer.android.com/reference/android/R.attr.html#protectionLevel)
