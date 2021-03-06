# Learn the Rebuilding Way!---Under construction

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

```
;
typedef struct SEXPREC *SEXP;
```

So all objects have two import attributes (R for beginners) mode and length.

Methods are not part of the class definition.

```
struct sxpinfo_struct {
    SEXPTYPE type      :  TYPE_BITS;
    /* ==> (FUNSXP == 99) %% 2^5 == 3 == CLOSXP
     * -> warning: `type' is narrower than values
     *              of its type
     * when SEXPTYPE was an enum */
    unsigned int scalar:  1;
    unsigned int obj   :  1;
    unsigned int alt   :  1;
    unsigned int gp    : 16;
    unsigned int mark  :  1;
    unsigned int debug :  1;
    unsigned int trace :  1;  /* functions and memory tracing */
    unsigned int spare :  1;  /* used on closures and when REFCNT is defined */
    unsigned int gcgen :  1;  /* old generation number */
    unsigned int gccls :  3;  /* node class */
    unsigned int named : NAMED_BITS;
    unsigned int extra : 32 - NAMED_BITS;
};

struct vecsxp_struct {
    R_xlen_t    length;
    R_xlen_t    truelength;
};


#define SEXPREC_HEADER \
struct sxpinfo_struct sxpinfo; \
struct SEXPREC *attrib; \
struct SEXPREC *gengc_next_node, *gengc_prev_node

/* The standard node structure consists of a header followed by the
 node data. */
typedef struct SEXPREC {
    SEXPREC_HEADER;
    union {
        struct primsxp_struct primsxp;
        struct symsxp_struct symsxp;
        struct listsxp_struct listsxp;
        struct envsxp_struct envsxp;
        struct closxp_struct closxp;
        struct promsxp_struct promsxp;
    } u;
} SEXPREC
```
### The "essential bit" of each language or why I started caring about referencing

Both Python and R have some capacity to perform Object Oritentated Programming (OOP) and Functional "Style" Programming (FSP). However, clear preferences exist as Python was built from the ground up with OOP in mind, while R clearly aims for FSP.

As shown above, Pythonic objects have four noteable traits:

   -they ultimately are descendents of the PyObject;
   -they contain both the data and methods for that data;
   -objects are built from other objects by referencing those objects, picking up data and methods along the way;
   -each object records the numbers of objects referecning it.

These seem to be the building blocks of the "Pythonic Data Model" and explain three of the important traits of Python:

   -The Dunder attributes and magic methods, which come from the PyObject C structure and thanks to all objects being 
    descended from PyObject become common traits for all object throughout Python;
   -Python uses "reference passing", which refers to Python Methods and Functions using the current version of the objects .       that are supplied to them as inputs;
   -The garbage collector will only consider objects which are not referenced.
   
 In R the SEXPRECs are the basic language elements which build the Data structures, symbols/names, environments and functions. 
 This leads to the noteable attributes of R:
 
    -Environments are objects 
 
 R functions have the following traits:
 
 Functions are not automatically bound to a name
 Side effects are minimized thanks to the way that call by value works
 
 
 The R SEXPREC object however is used to construct object to handle the process of a function:
 
 SYMSXP, symbols
 EXPRSXP, expressions
 LANGSXP, language constructs
 ENVSXP, environments
 CLOSXP, closures





###The Python and R Virtual Machines (Yes R has one too, it is just optional) 

Python

The Python evaulator reads the supplied code and transforms it to bytecode 

```
dis.dis('x=10; y=2; z=x*y')

  1           0 LOAD_CONST               0 (10)
              2 STORE_NAME               0 (x)
              4 LOAD_CONST               1 (2)
              6 STORE_NAME               1 (y)
              8 LOAD_NAME                0 (x)
             10 LOAD_NAME                1 (y)
             12 BINARY_MULTIPLY
             14 STORE_NAME               2 (z)
             16 LOAD_CONST               2 (None)
             18 RETURN_VALUE
```

However the second a function is called you can see that "CALL_FUNCTION" seems to be a black box 

```
dis.dis('exp(2)')
  1           0 LOAD_NAME                0 (exp)
              2 LOAD_CONST               0 (2)
              4 CALL_FUNCTION            1
              6 RETURN_VALUE
```




###The distributions of R

R stores the distributions in 'src/nmath'. The basic structure of these distributions is to check the validity of the entered parameters and then use the log density of a distribution.
