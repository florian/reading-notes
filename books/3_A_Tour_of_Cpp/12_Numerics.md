## 12. Numerics

### 12.2 Mathematical Functions

- `<cmaths>` has all the standard stuff
- `<complex>` has the equivalents for complex numbers
- Errors are written to `errno`

### 12.3 Numerical Algorithms

- `<numeric>` for more common algorithms
- `accumulate`, `inner_product`,  `partial_sum`, `adjacent_difference`
- There are also more general functions that allow replacing the default operation (e.g. `+` for `accumulate`) with a custom function
- `iota` for numeric ranges

### 12.4 Complex Numbers

- `complex` is a template to make it possible to set the type of the real and imaginary components

### 12.5 Random Numbers

- Random number generators consist of two components:
  1. An *engine* that produces a sequence of random values
  2. A *distribution* that maps those values respectively
- E.g. `uniform_int_distribution` accepts an engine as an argument, such as `default_random_engine`
- `auto dice = bind(uniform_int_distribution{1, 6}(default_random_engine{})`
- Binding an engine to a distribution yields a random number generator

### 12.6 Vector Arithmetic

- `valarray` helps with multidimensional arrays
- It has support for stride access
