# Learn the Rebuilding Way!

As a child I loved to pull things apart to try to understand them and I see no reason as an adult not to apply the same principle to programming languages.

## Python and R

### Page 1: The Basic Objects 

Let's start with Python. Building the basic python object in C++ will probably require standard C libraries:

```
#include <iostream>
#include <stdio.h>
#include <stdlib.h>
#include <string.h> 
```
Next some aliases taken from pyport
```
typedef ssize_t Py_ssize_t;
#define PyAPI_DATA(RTYPE) extern __declspec(dllexport) RTYPE;
typedef Py_ssize_t Py_hash_t;
```

Typedef standards unsurprisingly for “type definition”, allowing a name to act as an alias for a data type. So “typedef ssize_t  Py_ssize_t;” stands for let Py_ssize_t represent ssize_t;

Ssize_t, from the GNU C library, is the size of blocks which can be be read or written in a single operation. It must be a signed type (i.e. -2 requires more space than 2);

#define allows macros to be named. Macros in turn define constant values.  E.g. #define AGE 10 means that AGE=10;

#define something(something_else) something_else is a macro which returns the same data type which enters it. E.g. #define AGE(int) int; printf(“%i”, AGE(1)); will return a 1

Extern has two impacts, first it allows variables to declared without being defined (i.e. no memory is assigned to the variable) and it allows all of the program to see the variable. e.g. “int var;” will both define and declare var, but “extern int var;” will only define, while “extern int var=10;” will declare, define and extend to the whole program.

RTYPE, I think this refers to the return type description

__declspec(dllexport), from MSND “The dllexport and dllimport storage-class attributes are Microsoft-specific extensions to the C and C++ languages. You can use them to export and import functions, data, and objects to or from a DLL.”, “In newer compiler versions, you can export data, functions, classes, or class member functions from a DLL using the __declspec(dllexport) keyword. __declspec(dllexport) adds the export directive to the object file so you do not need to use a .def file.”,”For C functions or functions that are declared as extern "C", this includes platform-specific decoration that's based on the calling convention. For information on name decoration in C/C++ code, see Decorated Names. No name decoration is applied to exported C functions or C++ extern "C" functions using the __cdecl calling convention.”

P_hash_t is a simple ssize_t

```
 #define _PyObject_HEAD_EXTRA            
struct _object *_ob_next;           
struct _object *_ob_prev;
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/AlekJSmith/Learn-the-Rebuilding-Way-/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
