## 8. I/O Streams

### 8.2 Output

- `<ostream>`
- `<<` to write to a stream object
- `cout` is stdout, `cerr` is stderr
- Chaining `<<` is possible: `cout << a << b;`

### 8.3 Input

- `<istream>`
- `>>` to write to a stream object
- `cin` is stdin. Whitespace is stripped by default
- `getline` to read entire lines

### 8.4 I/O State

- stream objects hold state
- Testing an object (in an `if` or `for`) returns results depending on the state
- This allows us to iterate over streams elegantly: `while(is >> i) { … }`

### 8.5 I/O of User-Defined Types

- We can overload `<<` and `>>` to work with user-defined types
- There are methods for setting the state of a stream (`failbit`, `badbit`)

### 8.6 Formating

- *Manipulators* can be used for formatting
- `cout << hex << 16;`
- To manipulate the entire stream, there are special methods, e.g. `cout.precision(5)`

### 8.7 File Streams

- Similary, there are streams for files. Nothing surprising here

### 8.8 String Streams

- `<sstream>, stringstream` for streaming strings
- *(Note: This seems to be just for convenience, not performance)*
- `template<typename T=string>` for setting a default
