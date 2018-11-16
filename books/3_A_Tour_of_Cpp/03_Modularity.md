## 3. Modularity

### 3.1 Implementation

- To distinguish between interface and implementation, we have function *declarations* and *definitions*
- These are kept in different places

### 3.2 Separate Compilation

- User code only sees declarations
- The definitions are in separate files and also compiled separately
- This enforces strict separation
- Header files (.h) for declarations
- Implementation in .cpp files which include the header file

### 3.3 Namespaces

- Namespaces group declarations that belong together
- Avoids name clashes
- `namespace X { … }`
- `X::…`
- Standard library uses the `std` namespace
- To make everything from a namespace directly accessible: `using namespace std;`
- `using` in header files is bad style

### 3.4 Error Handling

- `throw exception(message)`
- `try { … } catch(exception) { … }`
- `<stdexcept>` defines some exceptions, e.g. `out_of_range`, `length_error`
- Functions that never throw can be declared `noexcept`, e.g. `void fn () noexcept { … }`
- To rethrow the same exception, just `throw` can be used
- Exceptions report errors at run time, asserts can also report during compile time
- `static_assert(4 <= sizeof(int), msg)`
- `static_assert` can be used for everything that is expressed using constant expressions. It is often used for checking types of parameters in generic programming
