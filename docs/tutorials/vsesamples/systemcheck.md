# SystemCheck sample VSE

*SystemCheck* VSE demonstrates how a Windows system calls can be used to provide information to a calling VoltScript script, which is not normally available. This VSE will not build or run on Linux or other platforms.

## Classes

### SystemInfo

`SystemInfo` features two methods that populate its properties. `DiskCheck` queries Windows for information about the disk drives on the system. `SystemCheck` queries Windows for information the processor, version of Windows, and physical memory.

This information is available via the `SystemInfo` properties. There are too many statistics to return in a single object, and a script may only be interested in a few of them. Providing information through properties gives a script flexibility in how and where this information is used. A script could use the remaining free space to start a backup transfer of files, change behavior based on the operating system, or print multiple statistics in a single statement.

## Test script

A test script for `SystemInfo` is provided. It calls `DiskCheck` and `SystemCheck`, then prints the determined information.

On Windows, the VSE can be built and the test script run with this command:

```bat
.\build-SystemCheck.bat --release --test
```

The test script is located on `src/SystemCheck/test/voltscript/SystemCheck.vss`.
