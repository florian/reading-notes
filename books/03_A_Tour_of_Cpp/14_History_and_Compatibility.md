## 14. History and Compatibility

### 14.1 History

- Developed at Bell Labs
- > AT&T Bell Labs made a major contribution to C++ and its wider community by allowing me to share drafts of revised versions of the C++ reference manual with implementers and users. Because many of those people worked for companies that could be seen as competing with AT&T, the significance of this contribution should not be underestimated.
- > At the time, I was comfortable in about 25 languages, and their influences on C++ are documented
- It took 30 years (until 2011) to get concurrency support standardized and make it universally available

### 14.2 C++11 Extensions

- A lot of stuff was only introduced in C++11, e.g. `initializer_list`, `auto`, `nullptr`
- C style casts were deprecated in favor of explicit casts ( `static_cast`, `reinterpret_cast`, `const_cast`)

### 14.3 C/C++ Compatibility

- With minor exceptions, C is a subset of C++
- Well-written C programs work in C++. The differences are mostly due to C++'s stricter type checking
- At this point, C and C++ are more like siblings because they borrowed ideas from each other over the years and stem from the same languages
- Being a perfect superset is difficult, simply because new keywords were added
