## 5. Templates

### 5.1 Introduction

- `Vector` doesn't care what types of elements are stored in it
- It should be parameterized by a type, so that anything can be stored in it
- A *template* is a class or a function parameterized with a set of types or values

### 5.2 Parameterized Types

- Prefix `class` with `template<typename T>` to parameterize it with an arbitrary type `T`
- > It is C++'s version of the mathematical [...] "for all types T"
- We can then parameterize the class with the type: `Vector<int>`
- To implement a range-`for` loop, `begin` and `end` need to be implemented: `for(auto& x : vec) { … }`. The iterator keeps yielding the return value of `begin` until its value equals the return value of `end`
- Templates are a compile-time mechanism, so there is no run-time overhead
- Templates can take value arguments: `template<typename T, int N>`
- Template value arguments must be constant expressions

### 5.3 Function Templates

- Functions can be parameterized in the same way
- `std::accumulate` can be used for summing up elements of containers

### 5.4 Concepts and Generic Programming

- The most common use case for templates is *generic programming* as they provide *parametric polymorphism*
- *Concept*: First template argument is a container, the second is some kind of number

### 5.5 Function Objects

- *Function objects* (sometimes called *functors*) are templates that define objects that can be called as functions
- `operator()` can be used to overwrite the *function application* operator `()`
- Useful for passing around functions that hold state
- The compiler can still inline functions
- `[&](int a) { return a < x }` is a *lambda expression*
- Capture nothing: `[]`, capture everything: `[&]`, capture only `x`: `[&x]`, capture copy of `x`: `[=x]`

### 5.6 Variadic Templates

- Templates and functions can be defined to take an arbitrary number of argument sof arbitrary types. In that case, they are called *variadic*
- `template <typename T, typename... Tail>`
- `void f(T head, Tail... tail) { … }`
- `tail...` can then be used to refer to the remaining arguments

### 5.7 Aliases

- Alias: `using a = b;`
- E.g. `using size_t = unsigned int;`
