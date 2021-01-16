---
title: C language
category: C-like
tags: [Featured, WIP]
prism_languages: [cpp]
updated: 2020-12-30
weight: -2
---

Intro
-------------------------------------

From the K&R book:

> For most programmers, the most important change is a new syntax for declaring and defining functions. A function declaration can now include a description of the arguments of the function; the definition syntax changes to match. This extra information makes it much easier for compilers to detect errors caused by mismatched arguments; in our experience, it is a very useful addition to the language. (p. 2)

> C retains the basic philosophy that programmers know what they are doing; it only requires that they state their intentions explicitly. (p. 3)


Variables
-------------------------------------

- Automatic variables are variables inside a function, therefore local variables.
- External and static variables are always initialized to zero.
- Automatic variables without explicit initialized have undefined values.
- Definition refers to the place where the variable is created and assigned storage.
    int bar;
    int g(int lhs, int rhs) {return lhs*rhs;}
- Declaration refers to places where the nature of the variable is stated but no storage is allocated.
    extern int bar;
    extern int g(int, int);
- For external and static variables, the initializer must be a constant expression and the initialization is done once, before the program begins execution.

### Static

- The `static` declaration, applied to an external variable or function, limits the scope of that object to the rest of the source file being compiled.
- The `static` declaration applied to internal variables cause the variable to remain in existence rather than coming and going each time the function is activated. This means that internal `static` variables provide private, permanent storage within a single function.

### Enums

```cpp
enum months { JAN = 1, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC };
```

### const

```cpp
const int a = 2;
int *pa = &a; // this will make const on line 1 useless

const int *pa = &a;
*pa = 9; // can't assign

int *const pa = &a;
pa = &a; // can't change what it points to
```

Bitwise
-------------------------------------

### Operators

| Bitwise operators   |                |                                              |
| -----------------   | -------------- | -------------------------------------------- |
| `&`                 | AND            |                                              |
| `|`                 | OR             |                                              |
| `^`                 | XOR            |                                              |
| `<<`                | left shift     | `a << x` is the same as `a * 2^x`            |
| `>>`                | right shift    | `a >> x` is the same as `a / 2^x`            |
| `~`                 | 1’s complement | It will be represented as a negative number. |

### Examples

```cpp
a & ~8 // turn fourth bit off
a | 8 // turn fourth bit on
a ^ 8 // flip the fourth bit
```

```cpp
int a = 5, b;
b = a++; // this will set b to 5 and a to 6
b = ++a; // this will set b to 6 and a to 6
```

Bit-fields
-------------------------------------

```cpp
enum
{
    KEYWORD = 01,
    EXTERNAL = 02,
    STATIC = 04
};

// turn on EXTERNAL and STATIC bits in flags
flags |= EXTERNAL | STATIC
// turn off
flags &= ~(EXTERNAL | STATIC);
// test them
if ((flags & (EXTERNAL | STATIC)) == 0)
```

### Using a struct bit-field

```cpp
    struct
    {
        unsigned int is_keyword : 1;
        unsigned int is_extern : 1;
        unsigned int is_static : 1;
    } flags;

    flags.is_extern = flags.is_static = 1;
    flags.is_extern = flags.is_static = 0;
    if (flags.is_extern == 0 && flags.is_static == 0)
```

Fields may be declared only as ints, specify `unsigned` explicitly.

Unions
-------------------------------------

Union is a variable that may hold objects of different types and sizes.

```cpp
union u_tag {
    int ival;
    float fval;
    char *sval;
} u;

u.ival;
```

See: [SO: Why do we need C Unions?](https://stackoverflow.com/questions/252552/why-do-we-need-c-unions)

### Accessing individual bytes

```cpp
typedef union
{
    struct {
        unsigned char byte1;
        unsigned char byte2;
        unsigned char byte3;
        unsigned char byte4;
    } bytes;
    unsigned int dword;
} HW_Register;
HW_Register reg;

reg.dword = 0x12345678;
// reg.bytes.byte3 will be 56 (depends on endianess)
```

### Accessing individual bits of a byte

```cpp
typedef union
{
    struct {
        unsigned char b1:1;
        unsigned char b2:1;
        unsigned char b3:1;
        unsigned char b4:1;
        unsigned char reserved:4;
    } bits;
    unsigned char byte;
} HW_RegisterB;
HW_RegisterB reg;

reg.byte = 15;
// reg.bits.b3 will be 1
```

Function pointers
-------------------------------------

```cpp
int fun(int a) {
    return a * 2;
}

int (*fun_ptr)(int) = &fun; // & is optional
(*fun_ptr)(10); // * is optional, fun_ptr(10) also works
```

```cpp
void wrapper(void (*fun)(int)) // * is optional, fun(int) also works
{
    fun(2);
}
```

References
-------------------------------------

- [comp.lang.c Frequently Asked Questions](http://c-faq.com/index.html)
