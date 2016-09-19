# Package Manager 

For application level permission the package manager service plays a role regarding runtime permission checks.

## Permission Checking

- [Context.checkPermission()](https://developer.android.com/reference/android/content/Context.html#checkPermission(java.lang.String, int, int))
  - `root` (and maybe `system`) passes all checks
- [Context.checkCallingOrSelfPermission()](https://developer.android.com/reference/android/content/Context.html#checkCallingOrSelfPermission(java.lang.String)) 
  - Pretty much a convenience method as calls the above internally

## Call resolution

TLDR `ContextWrapper` -> `ContextImpl` -> `ActivityThread` -> `ServiceManager` -> `ServiceManagerNative` -> `IServiceManager` -> `BpServiceManager` + the jni stuff -> binder ~> `PackageManagerService`
