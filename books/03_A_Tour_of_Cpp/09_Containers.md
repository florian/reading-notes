## 9. Containers

### 9.2 `vector`

- vector: self-growing array
- Pointers are user-defined types
- Elements are copied into vectors
- Vectors do not throw out-of-range errors on `[]` while they do on `at`
- Sometimes compilers offer range checking for vectors

### 9.3 `list`

- `std::list` for linked lists
- Every stdlib container provices `begin` and `end` for iterating
- `list<T>::iterator` (the return value of `begin` and `end`) can be used to refer to positions
- `insert` and `erase` take as input such iterators

### 9.4 `map`

- Standard dictionary
- Implemented as balanced binary tree
- `[key]` returns value or default value (`0` for `int`)
- `find` or `insert` can be used to avoid entering default values into the dictionary

### 9.5 `unordered_map`

- `map` requires a comparison function, `unordered_map` does not
- Works using hashing
- The standard library provides hashing for most types
- `hash<T>(expr)`
- To implement your own hash function, overwrite `operator()`. Alternatively, `std::hash` can be overloaded

### 9.6 Container overview

- There are also containers for single linked lists (`forward_list`), sets (`set`), multimaps (`multimap`)
- Many containers accept `push_back`
- Design advice: `()` initializer for sizes, `{}` initializer for lists of elements
