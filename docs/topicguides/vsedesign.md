# VSE design considerations

As you design your VSE, you may want to keep these design considerations in mind.

## The object model

Before you can design a VSE that implements an abstract model of objects and object relationships, you have to specify that model itself. And before that model can be specified, you need to specify the real-world objects and relationships that you mean to approximate with the abstract object model. This is "entity and relationship modeling": creating the first statement of what it's in the real world that you want to automate in an object-oriented program.

Some business operations of a typical company, for example, will involve aggregate objects usually called "cost centers" and "workgroups". 

These two kinds of real-world objects may be contained one within the other in the company, may be identical, or may be members of separate hierarchies with the same root. Although less likely, they may be completely unrelated. However, these two items are defined and related in the company, that needs to be understood before you can enter them faithfully into an abstract model.

The real-world relationships that you model will depend also on the kind of information you want the entire system to produce. For example:

- If the operations environment is an assembly plant in a manufacturing company and you want to model the manufacturing costs, then you may need to see each subassembly as an aggregate of its component parts, each with a cost of its own.
- If on the other hand you want to model the flow of work in progress through the plant, then the defining information about a subassembly may be not its component parts and quantities (the bill of materials), but the set of higher-level assemblies in which this subassembly is a component. 

Creating the abstract model means making decisions like these about objects and their attributes.

Note that this sort of modeling happens before designing your program. The result may be the model that the program will implement, or not. For example, suppose that in your company, workgroups and cost centers are the same operating units. But you think of certain attributes and operations for workgroups in your company, and other attributes and operations for cost centers. A model of all company operations can include these two kinds of objects, and for each, its attributes and operations. A VSE that implements the corresponding classes separately may be easier to understand and maintain than one in which a single class is used to represent both conceptual objects, together with the aggregate of all operations associated with either one. It's the VSE designer's choice to make, either implementation may serve its purposes better.

In the eventual VSE program, the C++ code that defines classes must of course represent some coherent conceptual model of objects and their relationships. Further, when the VSE registers these classes with VoltScript, the resulting classes are treated as distinct classes in scripts.

!!!note
    - The preceding description is not related to your choice of whether to use the VSID compiler to generate C++ class-definition structures to be accessed by your VSE and used in the calls to VoltScript that register the classes. The class-definition input to VSID is one-to-one with the resulting class definitions in C++.
    - The source-code example contents for VSEs in VSID are already realizations of simple conceptual object models. VSID doesn't help you directly with the design process of conceptualizing your real-world objects and designing the final object model that your VSE will implement. It only provides the tools to realize the final object model in code.

## The object creation model

You must design the mechanism for creating objects of LSX classes. There are two basic styles of how to create an object of a particular class:

- The class definition has a New method for objects of that class. An object of the class is created by invoking the exposed New method.
- An object of a class is created by running a create method for that class on an object of its parent class. The parent class will be the direct parent of the given class.

<!--Here are the details.-->

### Instantiation of an object using the New method

Suppose that the class definition of the class OutClass, say, exposes a New method. Then an example of the VoltScript language for creating a new object of class OutClass is:

`Dim XO As New OutClass`

This line of code does the following:

- It declares the variable XO whose type is the class OutClass
- It creates a new object of that class
- It stores in XO a reference to the new object

### Instantiation of an object using its creation method in a parent class

Suppose that an object of the OutClass class is created by running its creation method on an object of a parent class. Then an example of the VoltScript language for creating a new object of class OutClass could be:

`XO = XS.CreateOutClass`

In this line of code:

- XS is a variable referring to an object of the parent class
- CreateOutClass is a method in the parent class
- CreateOutClass creates an object of the OutClass class

One way to implement this model is to design the LSX classes such that:

- A Session class is parent of every other class; and 
- For every derived class, the Session class contains a create method for objects of that derived class

## The data model

You need to consider what kinds of data structures to use to represent the data members of the classes in your model.

- VoltScript supports array variables and list variables, as well as scalar variables of various data types.
- VoltScript also implements the Variant data type.
    - This data type enables the user to create multi-level list and array structures. 
    - For example, VoltScript supports an array of arrays, which is an array variable whose members are themselves arrays.

You can declare and use the data members of your VSE classes as these data types and structures.

### Collection classes

A further way to structure your LSX class is to define and register it as a collection class.

A collection class is a container of items that LotusScript can access directly in two ways:

- By indexing - This accesses an individual item in a collection.
- By the ForAll language iteration construct - This accesses every item in a collection.

The allowable operations are to access the values, the properties, and the methods of an individual item or of every item in the collection.

### Expanded classes

Define your VSE class as an expanded class if you want to omit the definition of one or more properties or methods when you register the class in VoltScript, and complete the class definition at runtime.

At runtime, a script statement that refers to an unknown member of a registered class triggers a callback to the VSE with the message `LSI_ADTMSG_BINDMEMBER`. The VSE code responding to this callback must supply the appropriate property or method definition.

This implementation of delayed binding allows you to design class definitions that vary depending on runtime conditions. To do this, you must register the class containing this property as an expanded class.

## Character sets

You must determine what character set to use to represent VSE-maintained strings that must be passed to VoltScript.

The VoltScript internal representation is UNICODE. However, an VSE or an embedding application can specify any of four "string communication" representations to VoltScript: 

- The platform-native character set
- UNICODE
- LMBCS, or the Lotus Multi-Byte Character Set
- ASCII

This means that a string will be presented to VoltScript in that representation. VoltScript is responsible for converting the string to UNICODE as needed for its own purposes. 

## Multi-platform design

You need to decide whether to write the VSE for one platform or several. Although single-platform design allows you to write C++ source code to take advantage of known compiler idiosyncrasies, the resulting source code is not portable.

In VSID, the platform-common code file `lsxcomm.cpp` is platform-independent. This file selects and includes a platform-specific header file. The selection criterion differentiates the platforms 32-bit Windows, 64-bit Windows, or Linux. The inclusion conditionalizes the compilation of the rest of `lsxcomm.cpp` for each platform.

Each sample provided with VSID compiles for all platforms that support the sample. For example, note the subdirectories for the Intel platforms w32 or w64 under `lsx\src\Datatypes\objs`. The compiler MAKE files instruct the compiler to output the object files in one of these directories, depending on the platform.

## Using GUIDs

A GUID-style class identifier must be used for any class that a VoltScript client, such as a VSE, exposes to VoltScript.

Your VSE should not use, for its classes, any GUID that's not in a range established as available for your use. Any other GUID might be used already, or in the future, by someone else, for a class exposed by some other application. Conflicting uses of a GUID can have unpredictable results. This is unlikely, since VoltScript will not allow a VSE to register a class that has the same GUID as an already-registered class.

The GUIDs for your VSE classes are automatically generated by VSID when you create a new project. You can also generate your own by running the GUIDGEN or UUIDGEN utility directly, or by request to Microsoft. These GUIDs are guaranteed non-conflicting with GUIDs for any other classes.