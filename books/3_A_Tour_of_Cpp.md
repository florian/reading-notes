# A Tour of C++ [[website]](http://www.stroustrup.com/Tour.html)

## 1. The Basics

### 1.2 Programs

* The compiler produces object files which are then combined by a *linker*
* Executable programs are not portable, source code can be
* The C++ standard differentiates between language features and the standard library

### 1.3 Hello, World!

* Curly braces express grouping
* Every program needs a global function called `main`
* If no value is returned, the program execution is considered to be successful. Nonzero values mean something went wrong
* `#include <iostream>` is an instruction to the compiler to include the respective library
* `a << b` writes `a` to the stream `b`
* `std` is the standard-library namespace
* `using namespace std` makes everything in the `std` namespace globally visible

### 1.4 Functions

* *Function declarations* state the name of the function, the return type and the types of the arguments: `double sqrt(double);`
* Implicit conversions might happen: `sqrt(2)` will use `2` as a double
* Function declarations can contain argument names but those are just for better readability, they are ignored by the compiler
* If a function is member of a class, the class is also mentioned in the function declaration: `char& String::operator[](int index);`
* *Function overloading*: If two functions have the same name but take different arguments, the compiler chooses the appropriate one for any given function call

### 1.5 Types, Variables, and Arithmetic

* Every name and expression has a type
* A *declaration* introduces a name and specifies its type
* An *object* is memory that holds a value of some type
* A variable is a named object
* A `char` holds a character. Its size is machine-dependent but often 8 bits are used. Sizes of all other types are defined in multiples of chars
* `sizeof(char)` is 1, `sizeof(int)` often 4
* There are multiple ways of expressing initialization
    * `double d1 = 2.3`
    * `double d2 { 2.3 }`
    * `=` automatically converts, `{}` errors out 
    * `int i1 = 7.2` will just store `7` while `int i2 { 7.2 }` produces an error
    * This is for C compatibility
    * The `=` in `int i3 = { 7.2 }` is ignored
* Constants cannot be left uninitialized, variables can be (`double d3;`). Generally it is good practice to only introduce a name once you have a value for it
* `auto` deduces the type from the initializer: `auto d4 = 7.2`. It avoids redundancy but should not be used if we want to make it clear what the type should be
* Advice: Prefer `{ }` if not using `auto`

### 1.6 Scope and Lifetime

* Local scope: Declared inside a function and only lives inside the `{ }` block
* Class scope
* Namespace scope: Defined in a namespace
* Global namespace: Anything else
* `new` creates objects without names
* Objects are destroyed at the end of their scope. Objects created using `new` live until destroyed by `delete`

### 1.7 Constants

* `const`: Value cannot be changed. Primarily used to define interfaces where functions do not modify input data
* `constexpr`: Should be constant at compile time. Functions need to be defined with this in mind. This is sometimes required by language rules, e.g. for array bounds

### 1.8 Pointers, Arrays and References

* `char v[6]` defines an array of six characters
* Arrays need to have a constant size (`constexpr`)
* `char* p` defines a pointer
* `char* p = &v[3]` points to the fourth element
* `int v2[3] = { 1, 2, 3 }` defines an array with values
* For iterating through it: `for (auto x : v) { }`. This copies each element of `v` into `x`
* If we want to iterate over references: `for (auto &x : v) { }`. This allows us to change the values of `v`
* A reference is similar to a pointer except that we don’t need to use `*` to access the value
* `*` means *contents of*, `&` means *address of*
* References cannot be changed to different objects after initialization
* If there is no object to point to yet, we can initialize pointers using the `nullptr`. There is only one `nullptr` shared by all pointer types
* In old code, `NULL` or `0` were used in place of the `nullptr`

### 1.9 Tests

* `<<` is *put to*
* `>>` is *get from*
* `count`, `cin` are the standard output, input streams
* A declaration can appear anywhere a statement can
* `switch` is pretty standard
