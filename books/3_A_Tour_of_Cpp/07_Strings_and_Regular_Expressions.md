## 7. Strings and Regular Expressions

### 7.2 Strings

- `std::string`
- Concatentation using `+`
- Strings are mutable, subscripting is supported
- Strings can be compared using `==`
- *Short-string optimization*: Short strings are kept in the `string` object itself, longer ones are stored on the heap
- `string` is an alias for `basic_string<char>`. For Japanese we could use `basic_string<Jchar>`. Similarly, we could handle unicode

### 7.3 Regular Expressions

- `<regex>`, `std::regex`
- `regex pat(R"(\d{5})");`
- `R"()"` is a raw string literal. This allows us to use backslashes more easily
- `regex_match`, `regex_search`, `regex_replace`, `regex_iterator`, `regex_token_iterator` (iterate over non-matches)
- Regular expressions are compiled into state machines at run-time
- `?` after a repitition notation means lazy / non-greedy matching: Use the shortest possible match
- *Max Munch rule*: Use the longest match by default
- Some character classes have names: `alpnum`, `alpha`, `space`, etc. These have to be bracketed using `[: :]`, e.g. `[:alpnum:]`
- Anonymous groups: `(?…)`
