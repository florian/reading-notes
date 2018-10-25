# MapReduce: Simplified Data Processing on Large Clusters [[pdf]](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)

*Dean, Jeffrey, and Sanjay Ghemawat. "MapReduce: simplified data processing on large clusters." Communications of the ACM 51.1 (2008): 107-113.*

MapReduce is a programming model, designed to make it easy to automatically parallelize programs.
Users implement the parallelized part of their program using the `map` and `reduce` functions, known from functional programming.
The `map` function takes a (key, value)-pair and produces zero or more new (key, value)-pairs.
The `reduce` function takes a key and a stream of all values assigned to the respective key, which is then used to produce new values belonging to the given key.
Many programs can be elegantly expressed using these two functions.
Since they are meant to be pure functions, without side-effects, the MapReduce library can automatically parallelize many parts of the computation.

## 1. Introduction

- Google developers have written many programs for processing large amounts of data
- These programs are often conceptually simple but have to deal with a lot of data
- To finish in a reasonable time, they need to be executed in a distributed way across many machines
- A lot of logic for distributing the computation and data and for handling machine failures needs to be implemented
- *MapReduce* is a new abstraction that is meant to hide all these details
- The fundamental idea is that many computations can be written using just the `map` and `reduce` functions from functional programming
- Users write their programs using these two functions and the MapReduce library can then automatically execute their code in a parallelized fashion across many machines
- The contribution of this work is (a) the programming model (b) a high-performance implementation of this model

## 2. Programming Model

- The MapReduce part of the programs takes a set of (key, value)-pair and produces a new set of (key, value)-pairs
- `map` takes an input pair and returns a set of intermediate (key, value)-pairs
- MapReduce groups these intermediate results by the keys
- `reduce` takes a key with a stream of values belonging to that key, and produces a set of new values for the given key
- The stream (or iterator) allows handling a list of values that is too large to fit in memory
- The `reduce` function often compresses this stream of values down to a single value (e.g. it adds all of them up)

### 2.1 Example

- Example problem: Given a text file with one word per line, count how often each word occurs
- `map`: Take each word and emit it as a new key with a value of `1`
- `reduce`: Since the library already grouped everything by word (since those are now our keys), we now only need to add up the values and emit new `(word, count)`-pairs

### 2.2 Types

- `map`: Takes `(k1, v1)` and produces `[(k2, v2)]`. Note that it can return several pairs and that they are from a different domain than the input
- `reduce`: Takes `(k1, [v2])` and produces `[v2]`. Note that the domain of the values stays the same and that the key is fixed (we return no new key)
- The C++ MapReduce implementation uses strings for keys and values. Users can cast them inside the `map` / `reduce` functions if necessary

### 2.3 More Examples

- Distributed grep: `map` emits a line if it matches the pattern. Essentially `map` acts as a filter here. `reduce` is the identity function
- Count of URL Access Frequency: Just like the word count example
- Reverse Web-Link Graph: `map` emits `(target, source)` pairs and `reduce` concatenates all the `source`s for each target
- Term_Vector per Host: Like Reverse Web-Link Graph with some extra filtering
- Inverted Index: Like Reverse Web-Link Graph but with words and document IDs
- Distributed sort: `map` emits `(key, record)`-pairs. `reduce` is the identity function. This works because MapReduces sorts between using those two functions

## 3. Implementation

- Many different implementations of this interface are possible. For example, one could develop an implementation for a single machine or one for use on a cluster
- Google has clusters with many commodity PCs connected together over Ethernet
- Because a cluster could have thousands of machines, failures are common

### 3.1 Execution Overview

- The input data is partitioned into *M* splits. These splits are then processed in parallel by many different machines. Each machine applies the `map` function to the data
- Even `reduce` can be distributed: The intermediate keys space is partitioned into *R* pieces using a hash function. Different machines can then apply the `reduce` function in parallel to value streams of different keys
- Typically each of the *M* pieces is about 64MB large
- The user program creates a master node as well as workers that are then managed by the master
- The workers directly read in the data themselves. This means that the data does not have to be passed in by the user program itself
- The `reduce` workers reads all intermediate data of their partitions and then sorts it by the keys
- Intermediate data is always written to disk
- In the end, the final result is available in one output file for each `reduce` task

### 3.2 Master Data Structures

- The master stores information about each `map` and `reduce` task, in particular whether it was already started or was completed
- The location of the intermediate data is always propagated through the master. This information is also stored

### 3.3 Fault Tolerance

- MapReduce is often executed on hundreds or thousands of machines, so it needs to be able to deal with failures
- The master pings workers periodically. If no response is received within a certain amount of time, the worker is considered failed. The task is then marked as not started and can be picked up by a different workers
- These tasks have to be fully re-executed because the intermediate results are not accessible by the master or other workers
- Reducers that depend on the intermediate data are told about the re-execution so that they know where to read the data from
- If the master fails, a new copy could be started from the last checkpointed state. This is not implemented because the master fails very rarely (There is only one master, compared to thousands of workers)
- When the user-supplied functions are deterministic, re-execution produces the exact same output. To implement this, all outputs of tasks are committed in an atomic way. `map` tasks write their output to a temporary file and. The location is shared with the master once the job is complete. If a master receives a message about an already completed job, it ignores it
- `reduce` tasks rename their temporary file once they are done. If the same `reduce` task was executed multiple times, the file system will directly handle the overwriting
- (*Note: I'm not fully sure why we need different solutions for `map` and `reduce`. Why couldn't renaming work for both kinds of tasks? Maybe it is because `map` stores the data on the current machine, while `reduce` writes it to a more easily accessible machine?*)
- Most `map` and `reduce` functions are deterministic. If they are not, there are weaker guarantees: The output is equivalent to the output of a sequential execution of the non-derministic program. However, there could be several of such executions. This is because we are not sure which intermediate results were read

### 3.4 Locality

- The Google File System is used
- Some optimizations are made so that workers read data from close machines

### 3.5 Task Granularity

- Ideally, *M* and *R* are much larger than the number of workers
- This improves load balancing and speeds up recovery when a worker fails (since its tasks can be distributed in a broader way)
- However, they should also not be too large because there is some scheduling overhead
- In practice, *M* is chosen so that individual tasks use 16 to 64MB of data. *R* is often chosen to be a small multiple of the number of used machines

### 3.6 Backup Tasks

- *Stragglers* are machines that need unusually long to complete a task. This could be because of hardware or network problems
- When the entire MapReduce operation is close to completion, backup executions are started for the remaining tasks. A task is then marked as complete if either the primary or the backup execution complete
- This significantly speeds up the entire execution. The sorting example in Section 5.3 takes 44% longer without this optimization

## 4. Refinements

### 4.1 Partitioning Function

- Users can influence how MapReduce partitions the data
- For example, one might want to ensure that all URLs with the same hostname are in the same output file

### 4.2 Ordering Guarantees

- Within each partition, the intermediate keys are processed in increasing key order
- This makes it easy to produce sorted output files

### 4.3 Combiner Function

- When the `reduce` function is commutative and associative, some optimizations are possible
- It can already be executed on the `map` worker to produce a reduced intermediate result
- This partial combining can significantly speed up some programs because less data is sent over the network
- Combiner functions are written like `reduce` functions but need to be explicitly declared as such

### 4.4 Input and Output Types

- The MapReduce library provides support for reading in various kinds of formats
- Users can implement a *reader* interface to use different formats

### 4.5 Side-effects

- Side-effects should be atomic and idempotent, e.g. by writing to a temporary file and then renaming it
- Atomic two-phase commits are not possible (think of multiple output files with cross-dependencies). This is not a problem in practice

### 4.6 Skipping Bad Records

- MapReduce can detect records that lead to the failing of jobs
- If users want, MapReduce can automatically skip those records in the future
- This might be desirable if an approximate result is fine

### 4.7 Local Execution

- There is an implementation of MapReduce that executes everything locally
- This makes development and debugging much easier
- Users can invoke their programs with a special flag to get more debugging help

### 4.8 Status Information

- The master runs an HTTP server that displays status information
- For example: How many tasks were completed / are in progress, links to stdout / stderr logs, an estimate of how much time the job still needs

### 4.9 Counters

- Counting things is a common use-case, so there is a helper for that
- Workers propagate their counts to the master, which aggregates everything
- The current counts are displayed on the status website
- Some counters are automatically maintained by the library itself, mostly for statistics (e.g. how many pairs were processed)

## 5. Performance

- All performance tests are done on 1TB of data
- 1800 machines with about 4GB RAM each, though about half of that is used by other jobs
- Results: It's really fast. The sorting example is impressive (beating the previous benchmark record significantly), even though there's not much MapReduce in there
- The first jobs generally finish within 90 seconds. Most of that time is spent organizing the cluster and distributing the code
- Backup tasks were extremely useful
- The system deals incredibly well with machine failures. They kill 200 workers after some minutes but there's only a very small performance penalty
- (*Note: This seems to be a paper that's a good example for impressive results*)

## 6. Experience

- The initial library was written and enhanced in 2003
- It's now used for many different things at Google and there are about 900 instances of MapReduce uses in the code base (2004)
- (*Note: Interestingly, machine learning is mentioned, even though the TensorFlow papers discuss that the MapReduce approach is not really optimal for ML*)

### 6.1 Large-Scale Indexing

- Google's web indexing job was rewritten to use MapReduce
- It now needs a lot less code, since much logic is now hidden by the MapReduce library
- This made adding new features significantly easier
- The performance is good enough to keep conceptually unrelated computations separate

## 7. Related Work

- Making it easy to parallelize things by enforcing a certain programming model is not a new idea
- Many concepts (e.g. backup tasks) were used in some form before
- However, with the exception of MPI primitives, none of the mentioned libraries seem to be popular anymore

## 8. Conclusions

- Google uses MapReduce for many different purposes
- The authors identify three reasons:
  1. The model is easy to use and hides many details
  2. A lot of problems can be expressed using MapReduce
  3. The implementation is incredibly performant
- Lessons learned:
  - Restricting the programming model makes many things easier and more elegant
  - Many optimizations for network bandwidth were necessary
  - Redundant execution can really reduce the time it takes to complete jobs

---

## Other notes

- The paper does not refer to the phase between `map` and `reduce` as *shuffling* like it is often done. I wonder where that term was introduced
