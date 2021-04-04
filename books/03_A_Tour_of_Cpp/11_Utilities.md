## 11. Utilities

### 11.2 Resource Management

- > A resource is something that must be acquired and later released
- Examples: Memory, locks, sockets, thread handles, file handles
- `mutex`: `lock`, `unlock`
- `unique_lock` can wrap a mutex. It locks it on initialization and releases it in the destructor
- Smart pointers `unique_ptr`, `shared_ptr` manage objects stored on the heap
- The object is released when the smart pointer is out of scope and gets destroyed
- `shared_ptr`: The object is destroyed when the last reference is out of scope. It uses copy semantics rather than move semantics
- `make_unique`, `make_shared` are helper functions for creating smart pointers
- `auto x = make_unique<A>(args)` is shorthand for `unique_ptr<A> x { new A(args) }`
- These wrappers make it possible to enforce a *no naked new* policy

### 11.3 Specialized Containers

- `T[n]` is a standard array
- `array<T, n>` wraps this for convenience and provides better type safety
- `bitset<n>`
- `pair<A, B>` for two values of arbitrary types
- `tuple<T, …>` for an arbitrary number of values with arbitrary types
- `make_pair` and `make_tuple` for creating pairs and tuples without explicitly stating their type
- `std::get<i>(tuple)` to retrieve the `i`-th element of a tuple. This provides type safety

### 11.4 Time

- `std::chrono`
- `time_point` for timestamps
- Difference of two `time_point`s is a `duration`
- Functions for casting between different time units

### 11.5 Function Adaptors

- `bind` for function currying
- `placeholders` contains `_1`, etc for declaring what arguments to set
- `bind(f, 2, _1)` sets the first argument to `2`
- If `f` is overloaded, we have to specify which one to use
- `mem_fn` lets us call a member function as a non-member. This is useful for passing it into other functions
- `auto draw = mem_fn(&Shape::draw); for_each(v.begin(), v.end(), draw)`
-  `function` lets us declare the type of a function. For example, `function<int(double)>` is the type of all functions that take a double and return an int

### 11.6 Type Functions

- *Type functions* are functions that take a type as input or return one as output. They are evaluated at compile time
- `numeric_limits` from `<limits>` provides the min/max values of certain types
- `sizeof` is another example
- Sometimes we want different implementations for iterators with different traits. `Iterator_category<IterationType>` constructs a tag that indicates what kind of iterator it is. We can then use function overloading with pattern matching to write fitting implementations
- `<type_traits>` lets us check for traits of types, e.g. whether they are arithmetic. This in conjunction with `assert` lets us declare what types are allowed in our templates
