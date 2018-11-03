# FlumeJava: Easy, Efficient Data-Parallel Pipelines [[pdf]](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/35650.pdf)

*Chambers, Craig, et al. "FlumeJava: easy, efficient data-parallel pipelines." ACM Sigplan Notices. Vol. 45. No. 6. ACM, 2010.*

Flume is a Google library for building and executing pipelines of MapReduce jobs.
Often, a single MapReduce job is not sufficient to compute everything.
Chaining several MapReduce jobs requires connecting them, cleaning up the intermediate results and implementing logic for handling failures.
Flume abstracts all of these details in a library.
It automatically optimizes pipelines and evaluates everything in a lazy way.
Conceptually, the interface and idea are similar to those of Spark.

## 1. Introduction

* Many tasks can be expressed ask MapReduce jobs, but often a sequence of such jobs is necessary
* Additional low-level logic is required to execute these pipelines:
    * Logic for connecting the jobs
    * Deleting the intermediate results in the end
    * Handling failures
* All of this makes it harder to change the pipeline
* Flume is meant to support the development of such data-parallel pipelines
* There are classes for representing *parallel collections*. These support *parallel operations*
* (*Note: This seems to be similar to Spark's RDDs*)
* Flume abstracts away the details of how the data is stored (in memory, in a file, in a database)
* Similarly, Flume abstracts away the details of how the parallel operations are executed
* For development, everything can be executed and debugged locally
* For good performance, evaluations are *deferred*. The invocation of a parallel operations does not execute anything but just records the operation internally
* Once the execution plan is constructed, Flume optimizes it
    * Chains of parallel operations are fused together
    * Depending on the size of the input data, parts of the pipelines are either executed locally or on a cluster
* After the execution, Flume cleans up intermediate results
* Performance is as fast as MapReduce programs optimized by experienced users

## 2. Background on MapReduce

* Flume builds on the MapReduce abstraction, which has three phases, each using the output of the previous one:
    * `map` invokes a user-defined function on each record independently and in parallel. Records consist of (key, value)-pairs. For each such input, the user-defined function emits zero or more new (key, value)-pairs that are written to an output file
    * The shuffle phase groups all pairs by the key and for each key then outputs a stream of its values. The user configures nothing in this phase
    * `reduce` takes each key and its stream, and emits the new values of the respective key. Often it reduces the stream down to a single value. If this function is commutative and associative, it can already be executed as a *combiner* in the map phase. This reduces the amount of data sent over the wire
* `map` and `reduce` can be the identity function
* These phases are designed in a way that they can be executed in a massively parallel way
* The MapReduce library deals with the low-level tasks of managing the intermediate data, distributing the computation and dealing with failures
* It's implemented in C++
* > The Map phase supports embarrassingly parallel, element-wise computations
* Using MapReduce means figuring out how to express computations in the map and reduce framework
* High-level tasks like "count the number of occurrences" need to be hand-compiled into low-level map and reduce tasks

## 3. The FlumeJava library

### 3.1 Core Abstractions

* Data
    * `PCollection<T>` is an immutable bag (multi-set) of elements, with a well-defined type
    * Optionally, collections can have order. In that case, we refer to them as *sequences*
    * Collections can be created from data in memory or by reading an input file
    * Datasets are often sharded into several files. Flume can read all of them at once
    * `PTable<K, V>` represents a (key, value)-table. It is a subclass of `PCollection<Pair< K, V>>` with some additional operations. In other languages, it might just be an alias for a collection
* Operations
    * Flume defines a few primitive data-parallel operations. All other operations are implemented using these building blocks
    * `parallelDo` applies a function element-wise to a `PCollection<T>` to convert it to a `PCollection<S>`. Each value is mapped to zero or more new values
    * There are subclasses of `DoFn` for common tasks, such as mapping and filtering. There is also a version that can produce multiple output `PCollection`s using a single pass over the data
    * `parallelDo` can be used to implement both `map` and `reduce`
    * Ideally, this function is pure and does not access global, mutable state
    * `groupByKey` converts a `PTable<K, V>` to a `PTable<K, Collection<V>>` by grouping the values belonging to each key. The value list is a plain Java collection. This corresponds to the shuffling phase in MapReduce and has to be used explicitly. There is a variant that allows specifying the exact sorting order
    * `combineValues` takes a `PTable<K, Collection<V>>` and converts it to a `PTable<K, V>`. Each value collection is reduced to a single value. This operation should be associative. It could be implemented using a `parallelDo` function but since the function has to be associative it can be executed faster by using a MapReduce combiner and reducer
    * `flatten` takes a list of `PCollection<T>`s and concatenates them. It does not actually copy any items but creates a view of them as one logical `PCollection`
    * The results of those functions can be combined using normal Java control flow, since the collections are regular Java objects

### 3.2 Derived Operations

* Several other operations are built on the primitive functions. They are no different to helper functions that users could write
* `count`: Maps each value so that it becomes a key with a value of 1, applies `groupByKey` and then adds up the counts using `combineValues`
* `join`: Merges two `PTable` so that values with the same key are grouped together. This could be used as an intermediate result for computing traditional inner and outer joins. The implementation is equivalent to that of a hashing-based join
* `top`: Takes a comparison function and a count `N`, and returns the top `N` elements according to the given comparison function

### 3.3 Deferred Evaluation

* Flume executes pipelines in a lazy way, using deferred evaluation
* Each `PCollection` is either marked as *deferred* or *materialized*
* When `parallelDo` or similar functions are called, they return a deferred collection
* The result of several such function calls is a directed acyclic graph that we refer to as the *execution plan*
* The user needs to call `FlumeJava.run()` to actually execute the commands. This optimizes the execution plan and then evaluates each node in a topological order

### 3.4 PObjects

* `PObject<T>` is a container that allows us to inspect values during or after the execution. It's a promise for a future value
* For example, `pcollection.asSequentialCollection()` returns a `PObject` that will eventually hold the respective data
* `getValue()` returns the value of the object, once it's available. Otherwise it seems to return something falsy
* The `operate` primitive can be used to execute code once a promise is fulfilled
* File-reading operations are overloaded to also be able to read filenames contained in `PObjects`
* `PObjects` can also be passed as side-inputs to `parallelDo` calls

## 4. Optimizer

* The entire optimization process fully consists of graph rewrites

### 4.1 `ParallelDo` Fusion

* If two `ParallelDo`s are composed, they can be fused into a single operation
* Siblings are also fused into an operation that has multiple outputs
* This also applies to `combineValues` operations

### 4.2 The MapShuffleCombineReduce (`MSCR`) Operation

* Combinations of `parallelDo`, `groupByKey`, `combineValues` and `flatten` are transformed into a single `MSCR` operation
* (*Note: This seems to be the abstraction for a single MapReduce job*)
* Such an operation has *M* inputs and *R* outputs
* Rules for reduce are less strict. It no longer needs to output values of the same key, since it's just a `parallelDo` internally

### 4.3 `MSCR` Fusion

* We consider `groupByKey` operations related if they make use of the same inputs
* These related operations form the basis for `MSCR`s as they define the inputs to the combined operation
* We move all M, S, C, R functions into individual `MSCR` blocks

### 4.4 Overall Optimizer Strategy

* The optimization performs several passes through the graph
* The goal is to minimize the number of `MSCR` operations
* The major steps:
    1. Sink `flatten`s. If we execute them as late as possible, we can fuse more `parallelDo` operations
    2. Move `combineValues` inside of `parallelDo`
    3. Insert fusion blocks. If we have an operation between two `MSCR`s, this marker determines which `MSCR` the operation is merged into. The fundamental idea here is that the output of each `MSCR` should be as small as possible, to minimize network traffic
    4. Fuse `parallelDo`s
    5. Create and fuse `MSCR`s

### 4.5 Example: SiteData

* Just an example of the optimized steps
* 16 operations are optimized into just two `MSCR`s

### 4.6 Optimizer Limitations and Future Work

* Code inside of functions is not analyzed or changed
* Traditional techniques, such as inlining code, could be applied here
* Users can provide hints about the size of the data, so that Flume can optimize in a more informed way. Static analysis might improve on this
* Common subexpression elimination could make sense
* Users often include `groupByKeys`, even though they don't need it. In some situations, this is because the next operation implicitly groups by keys anyways. The optimizor could detect this

## 5. Executor

* Once the execution plan is optimized, Flume runs it in a concurrent way
* Independent operations are executed in parallel
* If there's not enough data, Flume runs stuff locally to avoid overhead. Otherwise it launches a remote MapReduce job
* Flume automatically chooses the number of machines, based on prior analysis of the data and the operations
* Intermediate results are automatically deleted
* Design goal: Developing these pipelines should feel as if the developer was just writing a program that's locally executed. To help with this, Flume redirects all stdout/stderr from workers to the client and also rethrows errors there
* To aid with debugging, Flume can cache subresults. If a job fails, Flume can use these subresults and directly pick up the work that failed last time. This makes it much easier to iterate. Flume automatically detects what parts of the job changed and need to be reexecuted
* As of this paper, Flume only supports batch-mode and no streaming jobs

## 6. Evaluation

* The optimizer is extremely useful. In one example, it reduced 207 operations down to a single one
* It's on par with hand-optimization from experienced users

## 7. Related Work

* The optimizer uses many ideas from compiler optimization
* Before FlumeJava, there was a similar system with its own interface language
* That system was never widely adopted because it increased the effort necessary to understand everything (and some other reasons)

## 8. Conclusion

* Flume provides simple abstractions on top of MapReduce
* It uses a form of deferred evaluation to enable dynamic optimization
* The executor can choose among several implementation strategies (local, distributed)

---

## Other notes

* I'm not sure why this paper is so Java-centric. It seems like the paper would be much nicer to read if it would be just about Flume, not FlumeJava.
* Maybe the Lumberjack background story is the reason for the excessive use of Java
* Still, the Java generics make it easier to understand some things
* The related work section is absurdly long
