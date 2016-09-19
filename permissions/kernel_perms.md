# Kernel Permissions

## Overview

Used for low level components which dont have access to the `PackageManager`. A list of permissions handled in this way is below:

- Processes are assigned permissions based upon:
  - Its UID
  - Its GIDs
- Mappings are defined at:
   - Permissions -> GID: [data/etc/platform.xml](https://android.googlesource.com/platform/frameworks/base/+/master/data/etc/platform.xml)
   - Platform static UIDs and supplemental GIDs: [android_filesystem.config.h](https://android.googlesource.com/platform/system/core/+/master/include/private/android_filesystem_config.h)
     - _"The [3000](https://android.googlesource.com/platform/system/core/+/master/include/private/android_filesystem_config.h#109) series are intended for use as supplemental group id's only. They indicate special Android capabilities that the kernel is aware of."_
     - Also [maps AIDs (Android IDs?) to strings](https://android.googlesource.com/platform/system/core/+/master/include/private/android_filesystem_config.h#154)
- How GIDs are added to processes
  - For **static UIDs**, GIDs are assigned.. //TODO 
  - For **installed applications** (quick diversion) the package manager will add the GIDs for the application at install time, for permissions that appear in the `platform.xml` file, to `data/system/packages.list` for the applications entry
    - Entries appear in this list like `com.android.defcontainer 10003 0 /data/data/com.android.defcontainer platform 1028,1015,1023,2001,1035` 
- UID/GIDs are checked at:
  - Kernel/framework code level 
    - Where the perm is related to net activity regarding the AIDs in [linux/android_aid.h](https://android.googlesource.com/kernel/common/+/android-3.18/include/linux/android_aid.h) 
    - Or other AID restricted **(TODO:link to AID defs)** activity calling process UID/GID can be used to determine access
    - `Binder.getCallingUid()` inside services exposed a `Binder`. This is used when the callers UID is whitelisted in advance (i.e. if `root` or `system`)  
  - Filesystem level
    - Any file / folder access inc:
      - Where system daemons expose unix domain sockets via `/dev/socket/` (see `Appendix 1: Deamon Sockets`) as defined in [`init.rc`](https://android.googlesource.com/platform/system/core/+/master/rootdir/init.rc#617) (seems this config may have moved elsewhere)
  

## Appendix 1: Deamon Sockets
    
```
root@generic_x86:/ # ls dev/socket
adbd
dnsproxyd
installd
mdns
netd
property_service
rild
rild-debug
vold
zygote
```
  
## Links

An interesting example of inspecting a processes permissions can be seen here [Android Security: Welcome To Shell (Permissions)](http://doridori.github.io/Android-Security-welcome-to-shell/)
