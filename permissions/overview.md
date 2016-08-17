Permissions on android fall into two areas

1. How the OS handles permissions
2. How an app requests permissions

These will be outlined below

#1. Kernel Permissions

- Processes are assigned permissions based upon:
  - The owning user
  - Its PID
  - Its GIDs
  
An interesting example of inspecting a processes permissions can be seen here [Android Security: A Journey Into ADB shell permissions](http://doridori.github.io/Android-adb-shell-perms/)

#2. Application Permissions

_WIP_
