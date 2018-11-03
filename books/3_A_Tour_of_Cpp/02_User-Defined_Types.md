## 2. User-Defined Types

### 2.1 Introduction

* *User-defined types* are classes and enums
* *Built-in types* was everything previously introduced

## 2.2 Structures

* A *struct* defines a set of keys and their types
* `struct Vector { int sz; }`
* `Vector x;`
* We need to write a function to initialize a struct. This function should take a vector as a non-`const` reference
* `new` allocates memory on the *heap* (alternative names: *free store*, *dynamic memory*)
* Objects allocated there are independent of their scope and live until they are destroyed using `delete`
* `.` accesses `struct` members (keys) by name
* `a->b` is short for `*a.b` 

## 2.3 Classes

* If data is specified separately from operations, it can be used in various ways
* However, it is a good idea to keep the representation hidden from users so that it is easier to change it later
* We distinguish between interface and implementation
* `public:`, `private:` modify access
* `Vector v(s)` to initialize class by calling the constructor
* A function with the same name as its class is its constructor
* `operator[]` function to define the operator
* A `struct` is a `class` with all members `public` by default
* `struct`s can also have constructors and other member functions
* General design pattern: If you need a varying amount of space, point to somewhere else where you are allocating that space

## 2.4 Unions

* `union` is similar to a struct but only ever stores one of its members
* The programmer needs to keep track which one is currently stored
* Space efficient, e.g. if only one of two variables is used at the same time

## 2.5 Enumerations

* `enum`s define constants and group them under one name
* E.g. `enum class Color { red, blue, green}`
* Now we can use `Color::red`
* Under the hood it’s an integer, but that’s hidden. To us, `Color` is a normal type
* Operators can be defined for the type, e.g. `Color& operator++(Color& c) { … }`
* `enum` instead of `enum class` is an older, more low-level variant of it. Here, it is a bit more apparent that they are integers, e.g. they can be added up
