# Kernel Permissions

## Overview

Used for low level components which dont have access to the `PackageManager`. A list of permissions handled in this way is below:

- Processes are assigned permissions based upon:
  - Its UID
  - Its GIDs
- Mappings are defined at:
   - Permissions -> GID: [data/etc/platform.xml](https://android.googlesource.com/platform/frameworks/base/+/master/data/etc/platform.xml) (`/etc/permissions/platform.xml` on device).
   - Platform static UIDs and supplemental GIDs: [android_filesystem.config.h](https://android.googlesource.com/platform/system/core/+/master/include/private/android_filesystem_config.h)
     - _"The [3000](https://android.googlesource.com/platform/system/core/+/master/include/private/android_filesystem_config.h#109) series are intended for use as supplemental group id's only. They indicate special Android capabilities that the kernel is aware of."_
     - Also [maps AIDs (Android IDs?) to strings](https://android.googlesource.com/platform/system/core/+/master/include/private/android_filesystem_config.h#154)
   - Application UIDs
     - Can be seen at `/data/system/pacakges.xml` on rooted devices
     - Otherwise, `adb` `dumpsys package` as described [here](http://android.stackexchange.com/a/122757) 
- How GIDs are added to processes
  - For **daemon and native service processes**
    - init.rc [daemon setup](https://android.googlesource.com/platform/system/core/+/master/rootdir/init.rc#618)
    - deamon native code calling [libminijail.h](https://android.googlesource.com/platform/external/minijail/+/master/libminijail.h#44) methods e.g. the [adb daemon setup](http://androidxref.com/7.0.0_r1/xref/system/core/adb/daemon/main.cpp#113)
  - For **installed applications** (quick diversion) the package manager will add the GIDs for the application at install time, for permissions that appear in the `platform.xml` file, to `data/system/packages.list` for the applications entry
    - Entries appear in this list like `com.android.defcontainer 10003 0 /data/data/com.android.defcontainer platform 1028,1015,1023,2001,1035` 
- UID/GIDs are checked at:
  - Kernel
    - Where the perm is related to net activity regarding the AIDs in [linux/android_aid.h](https://android.googlesource.com/kernel/common/+/android-3.18/include/linux/android_aid.h) 
      - Example [net/ipv4/af_inet.c](https://android.googlesource.com/kernel/common/+/android-3.4/net/ipv4/af_inet.c#127) 
  - Framework
    - Calling process UID/GID can be used to determine access when interacting with system services
      - `Binder.getCallingUid()` inside services exposed a `Binder`. This is used when the callers UID is whitelisted in advance (i.e. if `root` or `system`)  
      - [PermissionState](https://github.com/android/platform_frameworks_base/blob/master/services/core/java/com/android/server/pm/PermissionsState.java#L434) objects which is used with the `PackageManagerService` contains GID info - as comment says this is not used often as generally if something has access the the `PackageManager` there is a cleaner API for app level permission checking. (see [Package Manager Permissions](package_manager_perms.md)) 
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
