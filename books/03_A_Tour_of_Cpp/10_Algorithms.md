## 10. Algorithms

### 10.1 Introduction

- To define `sort` behavior, `operator<` can be set
- A standard algorithm takes at least one half-open sequence of elements as input. This sequence is given by one iterator representing the first element of the sequence and by another iterator representing the one-beyond-the-last element
- `back_inserter` is an iterator that wraps a sequence and automatically expands space for writing to it. The respective container needs to support `push_back`

### 10.2 Use of Iterators

- `find` returns the one-beyond-the-last element if nothing was found
- Move semantics can be useful for algorithms that return a subset of the input sequence
- We can use templates or type aliases to create a function that takes an iterator as input
- > Iterators are used to separate algorithms and containers. An algorithm operates on its data through iterators and knows nothing about the container in which the elements are stored
- Functions with templates can have types defined explicitly or implicitly

### 10.3 Iterator Types

- Iterators are concepts and are implemented differently for various containers
- Containers provide the type of their iterator at `::iterator`

### 10.4 Stream Iterators

- Streams can be wrapped in iterators
- This allows for some elegant ways of reading from / writing to streams, e.g. to read values into a `set<string>`

### 10.5 Predicates

- Algorithms can accept predicates in the form of function objects or lambda expressions
- Example: `find_if`

### 10.6 Algorithm Overview

- `<algorithm>`
- `count`
- `equal_range` finds a range in a sorted sequence using binary search

### 10.7 Container Algorithms

- Sequences are defined as pairs of iterators `[begin, end)`
- To use `sort(v)` instead of `sort(v.begin(), v.end()`, we can overload `sort` ourselves or just use `Estd::sort` (e for extended)
- `sort` can also take a predicate that defines the sorting order
