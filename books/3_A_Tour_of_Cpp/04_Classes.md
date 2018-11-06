## 4. Classes

### 4.2 Concrete Types

- The default modifier is `private`
- Function inlining: Optimization technique where the code is copied to the place where it is called. It reduces overhead
- Functions of classes are inlined by default
- `inline` keyword (preceeding a function) to give the compiler a hint, which is mostly ignored nowadays
- *Containers* are objects holding collections of elements, e.g. `Vector`
- C++ defines an interface for garbage collectors, but not every environment has one and sometimes we need finer-grained control
- This is what *destructors* are for. They are denoted by `~` followed by the name of the class
- Memory is freed using the `delete` operator
- Code without explicit `new` and `delete` statements is less error-prone and easier to keep free of resource leaks
- `for(double d; is >> d) { … }` like a while but `d` has limited scope
- `std::initializer_list` is the type of lists that are used for initialization, e.g. `Vector v = { 1,2,3 }` creates such a list and passes it to the constructor of `Vector`
- `static_cast<type>(val)` gives us compile-time checking, which classic C casts do not provide

### 4.3 Abstract Types

- `virtual` means that a function may be redefined in a class deriving the current one
- `= 0` means it has to be redefined, it is *pure virtual*
- `virtual int size() const = 0`
- A class that provides an interface to a variety of other classes is called a *polymorphic type*
- `class A : public B { … }` means `A` derives from `B`
- If a function asks for `B`, passing `A` is also valid because it will have all the functions from `B`

### 4.4 Virtual Functions

- To know how function calls on objects are resolved, a *virtual function table* is created
- Each function is assigned to an index
- The cells of the table point to the respective functions
- Every instance of a class points to one table

### 4.5 Class Hierarchies

- Abstract classes should have virtual destructors
- `override` to let the compiler check that we are actually overriding something (like `@Override` in Java)
- > An object of a derived class can be used whenever an object of the base class is required
- `dynamic_cast<type>(expr)` to cast up, down or sideways in a class hierarchy. It can also be used to check the type, by inspecting the return value
- It is dangerous to return pointers to objects allocated on the heap. Maybe someone forgets to clean up. Instead, we can use `unique_ptr` to automatically free the memory when the object goes out of scope 

### 4.6 Copy and Move

- The default mechanism for copying objects is to copy all members
- This can fail in some cases, e.g. when attempting to copy a pointer. In that case, both the copy and the original would point to same element
- We can define our own *copy semantics* by implementing a *copy constructor* and a member for *copy assignment*
- Copy constructor: Constructor that takes its own type as input
- Copy assignment: Overriding `operator=`
- Sometimes we only want to move, not copy. For example, when returning an element from a function
- *Move semantics* allow us to implement behaviour for this. We override the same methods but the type is `Vector&&` instead of `Vector&` (for the example class `Vector`)
- Usually we copy the pointers and reset them in the object that we are moving them from
- `&&` means *rvalue reference*. rvalues are values that nobody can assign anything to, so it's save to steal values from them
- `std::move` casts to an rvalue reference, to enable moving it
- To explicitely generate default implementations for those constructors: `Y(Y&&) = default;` is the default copy constructor
- To only allow explicit conversions for constructors: `explicit Vector(int s);`. Now `Vector v(7)` is possible but `Vector v = 7` is not
- Garbage collection is a global memory management scheme. As systems get more distributed, locality becomes more important
- Excessive resource retention can be as bad as memory leaks, so it's good to be aware of how one is using memory
- *RAII (Resource Acquisition Is Initialization)*: Resources live as long as they are in scope
- Copy and move operations can be disabled using `= delete` instead of `= default`. This might make sense in base classes. Any operation can be surpressed like this
