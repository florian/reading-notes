## 13. Concurrency

### 13.2 Tasks and `thread`s

- `thread` takes a function as its first argument
- `join` to wait until the thread is done
- Threads share an address space, which also allows communicating between them

### 13.3 Passing Arguments

- The remaining `thread` arguments are passed to the function
- Alternative: Store them in a function object

### 13.4 Returning Results

- Use pointers and references

### 13.5 Sharing Data

- `mutex` and `unique_lock` as discussed before

### 13.6 Waiting for Events

- `<condition_variable>` to notify other threads that a value is now available

### 13.7 Communicating Tasks

- `promise` to pass values, `future` to accept them
- value or exception is passed to `promise`
- `future` waits for them
- `packaged_task` creates `promise` and `future`
- `async` to run a function in a different thread, without knowing how many threads are created. It essentially works on a higher abstraction level. A call to `async` returns a `promise`
