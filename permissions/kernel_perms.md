# Kernel Permissions

- //TODO include definitions in OS
- //TODO include exmaple call path for inet? (see blog post)

## Overview

- Processes are assigned permissions based upon:
  - Its UID
  - Its GIDs
- These are checked at:
  - Kernel/framework code level 
    - Where the perm is related to net activity regarding the AIDs in [linux/android_aid.h](https://android.googlesource.com/kernel/common/+/android-3.18/include/linux/android_aid.h) 
    - Or other AID restricted **(TODO:link to AID defs)** activity calling process UID/GID can be used to determine access
  - Filesystem level
    - Where system daemons expose unix domain sockets via `/dev/socket/` (see term output below) as defined in [`init.rc`](https://android.googlesource.com/platform/system/core/+/master/rootdir/init.rc#617) (seems this config may have moved elsewhere)
  

### Deamon Sockets
    
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
