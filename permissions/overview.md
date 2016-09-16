Permissions on Android fall into two camps:

1. Kernel Level (UID, GID)
2. App level (PackageManager)

The distinction between these are that kernel level permissions are needed for lower level permission granting and utilises OS level file permissions to regulate access.

//include definitions in OS
//include exmaple call path for inet? (see blog post)

App level permissions involve the PackageManager service.

//include info about where this runs and hows its accessed
//include info about meddling with definition files
//do some app permissions still get granted depending on UID and GID of app process? Check Sl.

//todo add /system/framework/..apk
