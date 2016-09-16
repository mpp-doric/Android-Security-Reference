# Package Manager 

For application level permission the package manager service plays a role regarding runtime permission checks.

## Call resolution

TLDR `ContextWrapper` -> `ContextImpl` -> `ActivityThread` -> `ServiceManager` -> `ServiceManagerNative` -> `IServiceManager` -> `BpServiceManager` + the jni stuff -> binder
