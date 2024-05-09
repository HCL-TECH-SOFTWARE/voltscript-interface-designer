# VSID internal code

## Generate Agents

### API Documentation

The agent generates API documentation for all project information in the VSID database. It processes all Script Extension Project and Script Library Project documents into an internal object model. It then merges generated output from that model with predefined template HTML content to produce HTML, CSS, and Javascript output files. The agent also produces a log with information about the processing and any warnings or errors that may have occurred. You can find the log in the **Logs** view. More log information is written to a text log file in the output API directory, when an exception occurs.

The object model and many of the utilities needed to generate this API documentation are in the *BSXdocumentationUtils* library. Code for loading information from various Documents, such as Project, Class, Method, is in the *BSXnotesUtils* library. Code for generating logs is in the *enhLogClassLite* library. Other libraries are also used, but these are the three primary "worker" libraries used by this agent.  

The predefined template HTML, CSS, and Javascript files are in the DESIGN of the VSID database, as File or CSS resources.  

### Extension Project Files

The agent generates the C/C++ source files needed to build a Script Extension Project. Unlike the other "Generate XXX" agents, this only runs on the selected Script Extension Project document and it's children. The output VSE directory can be specified using the **Setup** button in the **Script Extension Projects** view. Information from these documents is merged with predefined C/C++ Skeleton documents to produce the output. The agent also produces a log with information about the processing and any warnings or errors that may have occurred. You can find the log in the **Logs** view.

The primary utilities needed to generate this output are in the *LSXCodeGenerationScripts* library. The predefined C/C++ Skeleton files used for this agent are in regular Notes documents in **Administration** &rarr; **C/C++ Skeletons** view.

### Library Source Code Stubs

The agent generates VSS Source code stub files needed to build a Script Library project. It processes all Script Library Project documents into an internal object model, and then merges generated output from that model with predefined template VSS content to produce the VSS Script output files. The agent also produces a log with information about the processing and any warnings or errors that may have occurred. You can find this log in the **Logs** view. More log information is written to a text log file in the output API directory, when an exception occurs.

The object model and many of the utilities needed to generate this API documentation are in the *BSXdocumentationUtils* library. Code for loading information from various Documents, such as Project, Class, Method, is in the *BSXnotesUtils* library. Code for generating the logs is in the *enhLogClassLite* library. Other libraries are also used, but these are the three primary "worker" libraries used by this agent.  

The predefined template VSS files are in the DESIGN of the VSID database, as File or CSS resources.  

## Tools Agents

### Compute with Form

The agent performs a `ComputeWithForm()` operation on all selected documents.

### Fix Invalid App Names

The agent corrects invalid Application name values in all selected documents. This is only necessary if project documents were manually copied in from a preexisting LSX Wizard database.

### Generate Script Library API Documentation

The utility agent allows a user to select an external Script Library file, such as `.lss` or `.vss`, and try to generate API documentation in a similar manner as the *Generate API Documentation* agent. Code for parsing the external Script Library content into the object model is in the *BSXparsingUtils* library. 

### Import from Existing LSX Wizard

The agent queries the user for an existing LSX Wizard database, and generates documents in the current VSID database from the documents in the LSX Wizard database.  

### Log Config

The agent opens the Log Configuration document for editing to enable a user to specify various logging configuration options.

### Reset Ancestor ID Fields

When copying child documents, such as Class, Method, Variable, to new parents, the various ancestor ID fields, such as `F_ProjectID`, `F_ClassID`, `F_TypeID`, are no longer valid. Running the agent on these selected documents resets them to the correct values.

### Update Admin Docs From Chosen Template

Because some configuration information for the VSID application is stored in normal documents, such as C/C++ Skeletons, Keywords, DataTypes, this information needs to be updated after a design refresh from an updated template has occurred. The agent allows the user to select the appropriate VSID template and intelligently pulls in any updated information from the template documents.

<!--## API documentation site

[VoltScript Interface Designer Documentation](../apidoc/index.html){: target="_blank" rel="noopener noreferrer"} - API documentation including all libraries

### Specific Libraries

Libraries contained within VoltScript Interface Designer Documentation:

- [baliConstants](../apidoc/baliconstants_VSID/baliConstants_Library.html){: target="_blank" rel="noopener noreferrer"} - Constants used by VSID
- [baliCoreUtils](../apidoc/balicoreutils_VSID/baliCoreUtils_Library.html){: target="_blank" rel="noopener noreferrer"} - Core Utilities
- [baliExceptions](../apidoc/baliexceptions_VSID/baliExceptions_Library.html){: target="_blank" rel="noopener noreferrer"} -  Minimal Exception Tracking
- [baliListsCollections](../apidoc/balilistscollections_VSID/baliListsCollections_Library.html){: target="_blank" rel="noopener noreferrer"} - Lists and Collections Classes
- [baliStrings](../apidoc/balistrings_VSID/baliStrings_Library.html){: target="_blank" rel="noopener noreferrer"} -  String Utilities
- [baliUtils_nlsxBE](../apidoc/baliutilsnlsxbe_VSID/baliUtilsnlsxBE_Library.html){: target="_blank" rel="noopener noreferrer"} - NotesLSX Back End Utilities
- [baliUtils_nlsxUI](../apidoc/baliutilsnlsxui_VSID/baliUtilsnlsxUI_Library.html){: target="_blank" rel="noopener noreferrer"} - NotesLSX UI Utilities
- [BSXdocumentationUtils](../apidoc/bsxdocumentationutils_VSID/BSXdocumentationUtils_Library.html){: target="_blank" rel="noopener noreferrer"} - API Documentation UI Utilities
- [BSXnotesUtils](../apidoc/bsxnotesutils_VSID/BSXnotesUtils_Library.html){: target="_blank" rel="noopener noreferrer"} - API Documentation Notes Client Utilities
- [BSXnotesUtilsUI](../apidoc/bsxnotesutilsui_VSID/BSXnotesUtilsUI_Library.html){: target="_blank" rel="noopener noreferrer"} - VSID UI Utilities
- [BSXparsingUtils](../apidoc/bsxparsingutils_VSID/BSXparsingUtils_Library.html){: target="_blank" rel="noopener noreferrer"} - Parsing Utilities
- [DesignUtils](../apidoc/designutils_VSID/DesignUtils_Library.html){: target="_blank" rel="noopener noreferrer"} - Design Element Utilities
- [dxlUtilsBE](../apidoc/dxlutilsbe_VSID/dxlUtilsBE_Library.html){: target="_blank" rel="noopener noreferrer"} - DXL Back End Utilities
- [enhLogClassLite](../apidoc/enhlogclasslite_VSID/enhLogClassLite_Library.html){: target="_blank" rel="noopener noreferrer"} - "Lite" version of Enhanced Log Class
- [FileResource](../apidoc/fileresource_VSID/FileResource_Library.html){: target="_blank" rel="noopener noreferrer"} - File Resource Tools
- [LSXCodeGenerationScripts](../apidoc/lsxcodegenerationscripts_VSID/LSXCodeGenerationScripts_Library.html){: target="_blank" rel="noopener noreferrer"} - Scripts used for generation of LSX / VSE
- [LSXWizardScripts](../apidoc/lsxwizardscripts_VSID/LSXWizardScripts_Library.html){: target="_blank" rel="noopener noreferrer"} - Wizard Scripts for LSX / VSE processing
- [LSXWixardUI](../apidoc/lsxwizardui_VSID/LSXWizardUI_Library.html){: target="_blank" rel="noopener noreferrer"} - Wizard UI Tools

## Support

To share information, ask questions, and learn about VoltScript Collections, go to the [Community](https://support.hcltechsw.com/community?id=community_forum&sys_id=999cdacbdb82ed9055f38d6d13961961){: target="_blank"}.-->