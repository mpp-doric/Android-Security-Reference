Permissions on Android fall into two camps:

1. Kernel Level (UID, GID)
2. App level (PackageManager)

The distinction between these are that kernel level permissions are needed for lower level permission granting and utilises OS level file permissions to regulate access.

- //TODO include definitions in OS
- //TODO include exmaple call path for inet? (see blog post)

App level permissions involve the PackageManager service.

- //TODO include info about where this runs and hows its accessed
- //TODO include info about meddling with definition files
- //TODO do some app permissions still get granted depending on UID and GID of app process? Check Sl.
- //TODO add /system/framework/..apk
