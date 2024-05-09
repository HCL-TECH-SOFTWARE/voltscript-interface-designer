# VSEs and the VoltScript Interface Designer

The topic helps you understand what a VoltScript Extension (VSE) is, why you want to create one, and how you use the VoltScript Interface Designer (VSID) to do so.

## What is VSE?

VSE stands for VoltScript Extension module. A VSE is a shared library of VoltScript classes. It's implemented as a DLL, or the shared-library equivalent on non-Windows platforms. The source programming language for a VSE is C++.

A VSE is usable by the VoltScript language, which is an independent scripting language that can be run as a standalone executable, or as a part of the Foundry server.

A VSE can be portable. If the source is conditionalized appropriately, a single-source VSE module can be compiled and run on multiple operating systems. **The supported operating systems include Windows 32, Windows 64, and Linux**.

A VSE can also be sharable. A single loaded VSE can be accessed by multiple processes and execution threads. Thus, multiple instances of VoltScript can access the LSX simultaneously.


## What is VSID?

VSID stands for VoltScript Interface Designer. VSID is a toolkit based on the original LSX Toolkit first produced by Lotus Development for the Lotus Notes/Domino platform and related Lotus products that supported LotusScript. That original toolkit has been updated and evolved to support VoltScript, and new functionality has also been added to support VoltScript Libraries in a similar fashion.

The VSID includes:

- necessary VoltScript source files - These are principally header (.h) files that define the VSE API for the VSE builder
- C++ base class source code - This is intended for inclusion, unmodified, in your VSE sources
- Source code for VSE samples
- This documentation

The supported operating systems for this release of VSID are:

- 32bit Windows
- 64bit Windows
- Linux

## Why create a VSE?

A VSE enables the C++ developer to script classes defined in a wide variety of applications.

### VSE-defined classes

A script in VoltScript can manipulate several kinds of classes. The first two kinds are:

- Classes defined within VoltScript itself - These "native classes" are the classes defined by the **Class** statements within a script.
- Some LotusScript back-end classes, such as from Notes/Domino

A VSE provides a third kind of class that leverages the functionality of VoltScript. A VSE defines custom C++ classes using the published API of some other application, including possibly the API of an operating system. After loading the VSE by the VoltScript language engine, then:

- The classes can be registered with VoltScript in the same way as product classes.
- Scripts can manipulate these additional classes in the same way as the core classes.

The language used for operations on these classes is the same VoltScript language, used in the same way and with the same meaning as for core VoltScript classes.

In effect, VoltScript is being used to script a second application at the same time as the core language. The VSE provides an object model for some features of the scripted application, which is the second application.

### Functions in a VSE and in an existing DLL

Not only classes, but also global functions and constants, can be defined in your VSE and registered in VoltScript so that a script can invoke them. The typical VSE contains one or more of these.

However, if the functionality you need already exists as external functions in an available shared library written in C or in C++, then there may be no need to write and build a VSE. The following may be an alternative: 

In any script where you want to invoke such a function, you can use a VoltScript `Declare Function` statement to tell the VoltScript interpreter about the shared-library name and the function name, alias, and signature. Then the function is available thereafter to be used in the script. More commonly, you put the `Declare Function` statement in a script library and then put a `Use <library>` statement in any script where the function is needed.

### VSE development environment

To develop and test a VSE, the development environment installed on your workstation must include the following:

- A standard C++ development environment for the development platform. This includes:
    - A supported C++ compiler for your platform. It's recommended to use MS Visual Studio, even the Community Edition is fine. For Linux, it's recommended to install G++.
    - A C++ debugger
    - The platform's linker
    - A CMake utility
- An installed VSID 
- An installed VoltScript runtime environment