# VoltScript Extension samples

LotusScript&reg; features support for libraries, known as LotusScript&reg; Extensions or LSX's. These C/C++ binaries provide new API's that are accessed through standard scripts. VoltScript continues this feature with extensions known as VoltScript Extensions or VSE's.

HCL has developed a suite of [VoltScript Extensions](https://help.hcltechsw.com/docs/voltscript/early-access/references/vses.html){: target="_blank" rel="noopener noreferrer"} that provide VoltScript authors with many additional features not found in the base language. The VoltScript Interface Designer (VSID), evolved from the LSX Toolkit, creates new VSE's or imports existing LSX's. Once the design is complete, C++ skeleton code is generated, ready for implementation. This skeleton code handles communication with the VoltScript runtime, including registering the extension, receiving messages, and sending responses. Skeleton code compiles into a VSE that VoltScript can load, with methods taking no action when called.

Sample LSX's were included with the LSX Toolkit. Several of these have been updated as VSE's and are included with VSID. Developers interested in developing a new VSE can reference these samples to learn how to implement a VSE from the generated starter code.

These samples aren't intended as a "best practice" for implementing VSE's. Much of the code is not suitable for use in a production environment, without data sanitization, error checking, or robust and efficient data structures.

## Installation

An archive named `VSE-Samples.zip` is included in the VSID distribution. This archive includes the source code for the sample VSE's and build scripts.

Extract the contents of `VSE-Samples.zip` directly over the folder VSID is extracted to. No files will be overwritten. The samples include specially prepared build scripts and files, separate from the generated starter code. This samples archive depends on the common code included in the VSID distribution.

## Sample VSEs

### [Customer](customer.md)

Contains a simple `Customer` data class and a `CollCust` collection of `Customers`. The collection can be iterated through via the `ForAll` statement, and its contents accessed by index.

### [DataTypes](datatypes.md)

A VSE that features methods and properties of the native VoltScript data types. Methods provide examples of how to manipulate these data types as C++ objects and return them to the runtime.

### [LSXBEPlus](lsxbeplus.md)

A VSE that extends the LSXBE library from HCL Notes to show how to specialize some Notes back-end classes. In this example, `NotesDatabase` and `NotesView` are extended.

### [SystemCheck](systemcheck.md)

A VSE that uses Windows system calls to provide information about the host machine to the calling script through properties.
