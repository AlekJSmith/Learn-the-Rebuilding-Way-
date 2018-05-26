# Learn the Rebuilding Way!

As a child I loved to pull things apart to try to understand them and I see no reason as an adult not to apply the same principle to programming languages.

## Python and R

### The Basic Objects 

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

Struct A structure creates a data type that can be used to group items of possibly different types into a single type

*, returns a pointer to the memory address of an object

```
typedef struct _object {
    _PyObject_HEAD_EXTRA
    Py_ssize_t ob_refcnt;
    struct _typeobject *ob_type;
} PyObject;
```

Next R

Rather than taking an object and creating typed objects from it, the basic R object (SEXPREC) has a changeable attribute to define which data type it refers to.

```
typedef unsigned int SEXPTYPE;

#define NILSXP         0      /* nil = NULL */
#define SYMSXP         1      /* symbols */
#define LISTSXP         2      /* lists of dotted pairs */
#define CLOSXP         3      /* closures */
#define ENVSXP         4      /* environments */
#define PROMSXP         5      /* promises: [un]evaluated closure arguments */
#define LANGSXP         6      /* language constructs (special lists) */
#define SPECIALSXP   7      /* special forms */
#define BUILTINSXP   8      /* builtin non-special forms */
#define CHARSXP         9      /* "scalar" string type (internal only)*/
#define LGLSXP        10      /* logical vectors */
/* 11 and 12 were factors and ordered factors in the 1990s */
#define INTSXP        13      /* integer vectors */
#define REALSXP        14      /* real variables */
#define CPLXSXP        15      /* complex variables */
#define STRSXP        16      /* string vectors */
#define DOTSXP        17      /* dot-dot-dot object */
#define ANYSXP        18      /* make "any" args work.
Used in specifying types for symbol
registration to mean anything is okay  */
#define VECSXP        19      /* generic vectors */
#define EXPRSXP        20      /* expressions vectors */
#define BCODESXP    21    /* byte code */
#define EXTPTRSXP   22    /* external pointer */
#define WEAKREFSXP  23    /* weak reference */
#define RAWSXP      24    /* raw bytes */
#define S4SXP       25    /* S4, non-vector */

/* used for detecting PROTECT issues in memory.c */
#define NEWSXP      30    /* fresh node created in new page */
#define FREESXP     31    /* node released by GC */

#define FUNSXP      99    /* Closure or Builtin or Special */
;
typedef enum {
    NILSXP    = 0,    /* nil = NULL */
    SYMSXP    = 1,    /* symbols */
    LISTSXP    = 2,    /* lists of dotted pairs */
    CLOSXP    = 3,    /* closures */
    ENVSXP    = 4,    /* environments */
    PROMSXP    = 5,    /* promises: [un]evaluated closure arguments */
    LANGSXP    = 6,    /* language constructs (special lists) */
    SPECIALSXP    = 7,    /* special forms */
    BUILTINSXP    = 8,    /* builtin non-special forms */
    CHARSXP    = 9,    /* "scalar" string type (internal only)*/
    LGLSXP    = 10,    /* logical vectors */
    INTSXP    = 13,    /* integer vectors */
    REALSXP    = 14,    /* real variables */
    CPLXSXP    = 15,    /* complex variables */
    STRSXP    = 16,    /* string vectors */
    DOTSXP    = 17,    /* dot-dot-dot object */
    ANYSXP    = 18,    /* make "any" args work */
    VECSXP    = 19,    /* generic vectors */
    EXPRSXP    = 20,    /* expressions vectors */
    BCODESXP    = 21,    /* byte code */
    EXTPTRSXP    = 22,    /* external pointer */
    WEAKREFSXP    = 23,    /* weak reference */
    RAWSXP    = 24,    /* raw bytes */
    S4SXP    = 25,    /* S4 non-vector */
    
    NEWSXP      = 30,   /* fresh node creaed in new page */
    FREESXP     = 31,   /* node released by GC */
    
    FUNSXP    = 99    /* Closure or Builtin */
} SEXPTYPE

```


