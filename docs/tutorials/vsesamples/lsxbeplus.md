# LSXBEPlus sample VSE

*LSXBEPlus* VSE is an example of how to extend the existing HCL Notes classes. Support for these classes in LotusScript&reg; is provided via an LSX, LSXBE.

Much of the behavior is additional processing before or after calling the underlying LSXBE classes. Constants and definitions for these classes can be found in the VSID source code, on the path `src/inc/lsxnotes.h`. The classes from the inherited LSX are all defined as the type `ForeignLSXObj`. "Foreign" refers to an external VSE, different from the current one.

## Classes

### NotesDbPlus

This class extends the `NotesDatabase` class. It holds an internal `NotesDatabase` reference., which is called when needed.

#### Accessing an external VSE

A number of classes are defined in LSXBE, with the ID constants for their properties and methods in the `lsxnotes.h` header file. These classes and their properties and methods can be accessed from inside a VSE through the `ForeignLSXObj` class, which provides an interface to access other VSE's or LSX's via the VoltScript runtime.

For example, several methods require fetching a collection of all documents in the database. This is exposed as a property in the `NotesDatabase` class, named `AllDocuments`. `NotesDbPlus.cpp` accesses this property though the `GetProp` method of `ForeignLSXObj`, passing the property's ID constant:

```C++
NotesDocumentCollection DocColl = m_BaseNotesDatabase.GetProp(CNOTES_DBPROP_FINDALLDOCS).getForeignObject();
```

A VSE or LSX can define any number of new classes, which the calling class may have no knowledge of. As such, `GetProp` returns an extension of `LSXValue`, which represents a single object in the runtime. Depending on the type of the property, the appropriate getter for `LSXValue` should be called. `AllDocuments` is a `NotesDocumentCollection`, one of the classes defined in LSXBE, so its value is fetched via `getForeignObject`.

After fetching a collection of all documents in the database, the first document can be fetched using the `GetFirstDocument` method in `NotesDocumentCollection`. The `CallFunction` method with the appropriate method ID constant is used:

```C++
NotesDocument CurrDoc = DocColl.CallFunction(CNOTES_DCMETH_GETFIRST).getForeignObject();
```

`CallFunction` works similarly to `GetProp`, calling a method by its ID instead of accessing a property. As the `GetFirstDocument` returns a `NotesDocument` object, the `getForeignObject` method of the returned object is called to extract the `ForeignLSXObj` object.

### NotesViewPlus

This class extends the `NotesView` class. Like `NotesDbPlus`, it has an internal reference of the class it's extending.

#### Additional constructor

`GetViewPlus` in `NotesDbPlus` needs to construct a `NotesViewPlus` object with a different container object than usual. Refer to the comments for `GetViewPlus` in `src/LSXBEPlus/NotesDbPlus.cpp` for more details:

> When we specified the NotesView class in VSID, we declared the container class to be "default" (in fact, the Session object). We didn't specify the container as NotesDbPlus because we wanted the script writer to be able to access NotesViewPlus objects without needing a NotesDbPlus object. 

> The generated constructor contains code (in the base class) to add the new object to the Session's list of contained objects. However, in this case where the NotesDbPlus object is creating the NotesViewPlus object, we would really have this be the container object.

> Since the code was not generated that way, we need to add a new constructor to the NotesViewPlus class which will add itself to the right list, create an internal data member in this class to hold the list (do not forget to initialize it in the constructor) and create functions for manipulating this list that can be called by the NotesViewPlus class.

> We also need to add code to the NotesViewPlus' destructor to remove itself from this list when the object is destroyed, and code to this objects destructor to destroy the list and it's elements when it's destroyed.
