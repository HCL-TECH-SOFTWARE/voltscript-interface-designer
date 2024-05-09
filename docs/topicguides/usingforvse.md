# Using VSID for VSEs

The VoltScript Interface Designer (VSID) consists of a Notes database and an LSX that helps you to create a VSE. VSID compiles information about the VSE you want to create, generating a near-complete set of buildable files.

VSID is the tool you use in the first two steps of this four-step process that creates a VSE:

1. Create a *VSE project*, the set of all the VSE specifications.

    These specifications are documents in the Wizard's Notes database. You create them through the Wizard user interface.

2. Generate VSE files. 

    The VSID VSE does this when you invoke the action "Generate VSE Files" in the VSID UI.

3. Edit the generated C++ source files using your C++ design environment.
4. Compile the resulting source files to build the VSE.

    You can use one of the generated CMAKE files, the do_it batch file that invokes the C++ compiler on your workstation, or the VSID-generated Microsoft project file (.dsp).

    You can then test your VSE using the VoltScript runtime.

## Special features

In this release of VSID, you can:

- Generate VSE source files from specifications in the VSID Notes database and edit them, then change the specifications and re-generate the files from the new specifications, without losing the results of the editing.
- Import VSE specifications from earlier releases of VSID or the VSE Toolkit.
- Edit VSE specifications at several levels in the Notes user interface, such as at the Project level for the complete VSE, by individual documents, or by fields within documents.

## Generating VSE source files

The command **Generate Extension Files** appears on the Actions menu in the VSE Projects view in the VSID application. When you invoke this action with any document in a selected project, it generates the VSE source files from the specifications in all the documents for that project.

The complete set of files is generated each time. There is no partial file generation, even if some specifications are unchanged since the last generation. For a VSE of typical size, the generation takes a few seconds to create and write the files.

The generated files are written in the directory `vse\src\<vse_name>`, where `<vse_name>` is the name specified in the *Name* field of the Project document for this VSE. In Windows, for example, they include these files:

- Four files for each class defined in the VSE. If the class is named `<cname>`, the files are: 
    - `<cname>.hpp`
    - `<cname>.cpp`
    - `i<cname>.cpp` 
    - `i<cname>.tab`

    !!!tip
        The leading `i` in the names, such as in `i<cname>.cpp`, stands for *infrastructure*. For more information, see [Post-editing and tags](#post-editing-and-tags).

- `lsxsess.hpp` and `lsxsess.cpp` for the VSE's Session class
- Project-level files `lsxapplx.h`, `lsxapplx.cpp`, `itextstr.h`, and `iguidfile.h`
- CMAKE files, `.def` files, and batch files for use in building the VSE
- Library inclusion files, for example `libsresp.va2`

You can run the **Generate Extension Files** action on any of the sample VSE projects in the provided samples, and then examine the appropriate subdirectory of `vse\src` to see these results.

### Post-editing and tags

After generating the VSE source files for a particular project, you edit some of these files to add your implementation to the generated stubs.

You will routinely want to iterate through this three-step cycle:

1. Re-specifying the VSE in the VSID application
2. Generating VSE source files
3. Editing the generated VSE source files

For example, you may want to add a class to the VSE, or change a method's signature. When you complete any changes to the specifications in the VSID UI and run **Generate Extension Files** again, the contents of the directory `vse\src\<vse_name>` are completely replaced by this new round of generated files. To prevent losing the editing you did to the last preceding round of generated files, VSID uses a special mechanism. Certain of the files generated in any run contain tags to mark the locations in the file where you can post-edit. Any material that you insert at these marked locations is incorporated into the files generated in the next round. It's **protected**. VSID carries it over when writing the next version of the file.

Every tag is in two parts, a "begin tag" and an "end tag," in the form:

`//{{ VSE_AUTHOR_CODE_tagname`
`//}}`

The begin and end tags just shown are literal strings except for `tagname`, which is the name of any particular tag. Code that you insert between the begin tag and the end tag is protected. Thus, to ensure that your edits are preserved, you should edit the generated files only between the tags.

To acquaint yourself with this mechanism, browse through the files in the `vse\src\<vse_name>` directory for a sample VSE and look for this string:

`//{{ VSE_AUTHOR_CODE_`

You'll find that:

- The files `<cname>.hpp` and `<cname>.cpp` contain tags, where `<cname>` is the name of any class defined in the VSE. This includes the Session class automatically generated by VSID, represented by the files `lsxsess.hpp` and `lsxsess.cpp`.
- `lsxapplx.h` and `lsxapplx.cpp` contain tags.
- There are never tags in any file whose filename is prefixed with `i`, lower-case letter "i" for *infrastructure*. For example, no tags are generated in `ilsxsess.cpp` or `ilsxsess.tab`.

Of course, you can edit any VSID-generated source file at locations that aren't tagged, and you can edit any file that contains no tags, such as a file whose filename is prefixed with `i`.  However, such edits aren't protected and will be overwritten. The "i"-prefixed files are designed to need no editing.  

!!! note
    **Generate Extension Files** backs up the current generation of files when regenerating. Therefore, unprotected edits to the current generation aren't completely lost. But they aren't merged into the new generation of files like protected edits.

### Tags and tagged code in the VSE source files

In the tag string `LSX_AUTHOR_CODE_<tagname>`, the `<tagname>` identifies the likely purpose of code that you will write at that tagged location. For example:

- `LSX_AUTHOR_CODE_Include1` appears just before a standard set of `#include` statements near the beginning of `<cname>.hpp`. You would insert there any `#include` statements that should appear before the standard set.
- `LSX_AUTHOR_CODE_Private_Internal` appears just after the "private:" declaration near the beginning of a class declaration in `<cname>.hpp`.
- *Internal* refers to the fact that these declarations are only used internally by your VSE, and not accessible from VoltScript.

Three tags need special mention:

In the generated code, every class inherits from the abstract class LSXBase, as shown in `<cname>.hpp` and `<cname>.cpp`. VSID doesn't support multiple inheritance. But these two tags enable you to specify one or more additional classes for cname to inherit from:

- `LSX_AUTHOR_CODE_Additional_Base_Classes` in `<cname>.hpp` 
- `LSX_AUTHOR_CODE_Additional_Base_Class_Init1` in `<cname>.cpp`

You can initialize any variables by adding lines below this tag:

- `LSX_AUTHOR_CODE_Internal_Member_Init1`

If the class contains any properties, they're initialized in VSID-generated lines immediately following the tag. You add your initializations next. Each of your initialization lines following the property lines must begin with a comma. Usually, an initialization line would contain a trailing comma; but VSID doesn't generate that comma on the last property variable initialized, since it's unknown whether anything is to follow.

The set of VSID-generated tags is the same for every VSE, except that some tags are associated with a named property or method, and are based on the property or method name. Tags appear in the same approximate locations in the generated source files, again allowing for differences in the number and names of classes and their members.

#### Personal tags

You can also define your own *personal tags*, writing them into the generated source files. When VSID regenerates files, it will not preserve the location of a personal tag, but comments out the code between the begin and end tags and pastes the result at the bottom of the file.

When you create a personal tag, you need to follow these rules:

- Any personal tag must have the same format as a Wizard-generated tag:

    `//{{LSX_AUTHOR_CODE_mytagname`

    `//}}`

- `mytagname` must be unique: different from any VSID-generated tag and from any other personal tag
- You can use a personal tag only in a file that contains VSID-generated tags
- These are the only files that VSID checks for the presence of tags when re-generating files
- You cannot have nested tags

Most VSID-generated tags are empty: that is, there is no Wizard-generated code between the begin tag `//{{LSX_AUTHOR_CODE_tagname` and the end tag `//}}`. These are the exceptions:

- Method return values in any `<cname>.cpp`, following the begin tag `//{{LSX_AUTHOR_CODE_Method_<method_name>`
    - These are just dummy values so that the newly generated code will compile.
- Error-message code in `lsxapplx.h` and `lsxapplx.cpp`, following the begin tags `//{{LSX_AUTHOR_CODE_Error_Messages` and `//{{LSX_AUTHOR_CODE_Min_Error_Code`
    - There VSID generates a couple of default error messages for you, and a minimum error-message code. You are free to change them, or to add your own.

In all other cases, you create protected code by simply adding lines between the line `//{{LSX_AUTHOR_CODE_tagname` and the end tag line `//}}`.

## Regenerating VSE source files

When you run the action **Generate Extension Files** in the VSID user interface, it checks for the existence of the target directory `vse\src\<lsx_name>`.

- If this directory doesn't exist, VSID creates the directory, generates the VSE source files, and writes them into it.
- If the directory exists, VSID assumes that the VSE source files have been previously generated. Then, the generation code runs a two-step process:
    - Back up the existing files to the subdirectory `vse\src\<lsx_name>\backup`.
    - VSID creates the backup subdirectory if it doesn't exist already. If it does exist, the new backups overwrite any existing contents. Only one level of backup is maintained.
- Generate new source files into the directory, merging in the tagged code from the existing files.

The generate-and-merge process works as follows:

- A validation routine in the VSID Wizard DLL checks the existing files in `vse\src\<lsx_name>` to verify that all the tags in those files are valid.
    - For instance, every begin tag must have an end tag. Also, tags may not be nested.
    - Any errors found by the validation routine are reported to the user. The user must fix them before VSID generation will generate any code.
- If a method, property, or event has been renamed in the VSE specifications, any tag name in any old file that uses this name is modified to use the new name.
- Each new file is generated based on the VSE specifications. The file is generated with tags in it. Each tag is empty or contains default code.
- For each tag, the tag contents from the old file are merged with the tag in the new file.
    - If a method, property, or event has been deleted, the corresponding tag contents from the old file are commented out and the commented material is pasted at the bottom of the new file.

    