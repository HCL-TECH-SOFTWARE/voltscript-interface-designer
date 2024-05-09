# DataTypes sample VSE

*DataTypes* VSE contains a single class with a selection of data types as both properties, arguments, and return types. This serves as an example of how native VoltScript data types can be manipulated in a VSE, even if they do not directly map to a C++ data type.

## Classes

### DataTypes

This VSE features examples of how to receive, manipulate, and return common data types from the VoltScript runtime. Refer to the method comments in `src/DataTypes/Datatypes.cpp` for details for a specific data type.

#### Initializing currency data types

VSE's represent currency data types as structs, with `Hi` and `Lo` fields. These aren't automatically set in the generated constructors, and must be manually set by the author in the body of the constructors.

#### Manipulating Date objects

`DateDemoFunction` provides an example of how to manipulate Date objects. These are represented in C++ with the `LSXDate` class, which provides getters and setters for the fields of time. The runtime's serialization methods are used by the VSE when returning a `LSXDate` back to the runtime. Note that a raw float cannot be used to initialize a `LSXDate` the way a `Date` object can in a script.

#### Raising events to the runtime

A VSE can raise an event to the runtime for any reason a developer wishes, such as an error or warning. Events can be raised at any time during a script, even if the runtime is not currently making a call to the VSE.

Events are first defined in VSID. `DataTypes` defines three events, `NumberArgsDemoEvent`, `StringArgsDemoEvent`, and `ObjectArgDemoEvent`. Each of these events take different arguments, as suggested by their names. All three events are raised by `RaisingEventsDemoMethod`. 

Events can be handled in a script through the `On Event` statement. For example, the `StringArgsDemoEvent` can be handled by the following sample sub:

```VB
Sub StringArgsDemoHandler(byValArg as String, byRefArg as String)
    Print "Arugment by value: " & byValArg & ", argument by reference: " & byRefArg
End Sub
```

In this sample, the first argument passed to `LSXRaiseEvent_StringArgsDemoEvent` maps to `byValArg`, and the second argument maps to `byRefArg`. The second argument is passed by reference - changes made to it in VoltScript will be visible in the VSE, though the sample code does nothing with the variable after raising the event. 

## Test script

An example test suite that uses the VoltScriptTesting library is included with this VSE. These files can be found in the `src/DataTypes/test/voltscript` folder, and the script can be run using the `build-DataTypes` scripts.

On Windows, the VSE can be built and the tests run with this command:

```bat
.\build-DataTypes.bat --release --test
```

On Linux, the command is similar:

```bash
./build-DataTypes.sh --release --test
```

The `build-DataTypes.sh` script may need to be set as executable before it can run.
