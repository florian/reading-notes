# TensorFlow: A system for large-scale machine learning [[pdf]](https://www.usenix.org/system/files/conference/osdi16/osdi16-abadi.pdf)

*Abadi, Martín, et al. "TensorFlow: a system for large-scale machine learning." OSDI. Vol. 16. 2016.*

[TensorFlow](https://www.tensorflow.org) is Google's machine learning library.
It lets developers use the same code to train models in various environments, e.g. on clusters or personal computers.
Furthermore, it can compute gradients automatically, which makes experimenting with new kinds of models or optimizers much easier.
This paper explains some historical design decisions behind TensorFlow and describes the ideas behind its implementation.

## 1. Introduction
* TensorFlow can be used in various environments:
    * It efficiently uses large clusters of GPU-enabled servers
    * It can be used locally on mobile devices
* At the same time, it is meant to be flexible enough for research
* A dataflow graph represents the computation and the state of an algorithm
* Unlike in traditional, purely functional dataflow systems, nodes can own and mutate state
* Nodes correspond to computations
* Data in the form of tensors (multi-dimensional arrays) flows along edges
* TensorFlow allows programmers to experiment with different parallelization schemes

## 2. Background & Motivation

### 2.1 Previous system: DistBelief

* [DistBelief](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/40565.pdf) was Google’s previous system for training neural networks
* It used the *parameter server* architecture
    * There are two kinds of processes: Workers that perform only pure computations, and parameter server processes that maintain the parameters of the model
    * As outlined later on, this is not a very flexible approach
    * *(Note: This is not mentioned, but parameter servers also seem to break abstraction principles. I only want to describe the required computations, not how they are distributed on the cluster)*
* The system was not flexible enough:
    * New layers had to be defined in C++, even though the main interface was in Python. This made it more difficult to experiment with ideas and increased the entry barrier
    * The system was not flexible enough to change how the optimization algorithm works. To change update rules, one had to deal with the parameter server. Updating parameters in an atomic way was not possible
    * The system was not flexible enough to change how the training in general works. A training iteration always meant that some training data is passed through the model, gradients are computed, and parameters are updated afterwards. Sometimes, we want to work in more complex environments, e.g. when reinforcement learning agents interact with other programs. This fix approach also made it impossible to experiment with totally novel ways of training models
* DistBelief was designed for use in data centers. It scaled down very badly when trying to use it for prototyping on individual machines. This also made it more difficult to deploy models to production systems, e.g. to ship a model with an app

### 2.2 Design principles

* A high-level scripting interface allows users to construct graphs that can then be deployed on clusters
* Those graphs are composed of primitive operators, which makes it easy to create new kinds of layers
* There are two phases:
    1. Create the graph
    2. Execute the compiled graph on a set of available devices
* Execution is deferred, which allows TensorFlow to optimize the execution phase. This makes things more efficient but also means that control flow has to be moved into the graph
* A *device* is hardware that TensorFlow can execute graphs on. Devices need to implement a few fundamental operations. This abstraction allows TensorFlow to run graphs on many kinds of platforms, without the user needing to worry too much about whether they want to use CPUs, GPUs or TPUs
* Tensors are the primitive interchange format that all devices need to understand
* All in all, TensorFlow is designed to be flexible for users

### 2.3 Related work

* Many libraries that only work on single machines (such as Caffee, Theano, Torch) scale badly
* Batch dataflow systems, usually based on MapReduce, such as Spark require too much time for synchronizing parameters. Immutable state leads to inefficiency problems
* Parameter server based systems such as DistBelief or MXNet make experimenting difficult

## 3. TensorFlow Execution Model

* The graph represents all computation and state in the implemented ML algorithm
* This includes input processing and update rules
* Mutable state is crucial for training large models

### 3.1 Dataflow graph elements

* *Tensors* are dense, n-dimensional arrays with a specified data type
* Sparse tensors can be represented using dense tensors (e.g. by two tensors, one that contains indices with non-zero values, and one that provides their values)
* *Operations* are the functions that are executed in nodes. They have zero or more outputs and zero or more inputs. The output types and shapes do not have to be known at compile time
* *Variables* are nodes that maintain state between graph executions. They are operations that take no inputs and output a reference handle that can be used for reading and writing that buffer
* *Queues* are stateful operations that can be used to coordinate execution. There is a simple *FIFOQueue*, as well as a queue that randomly samples elements. Similar to variables, the queue operation returns a handle that can be used for enqueuing and dequeuing tensors. These handles block when the queue is full / empty. Queues can be used in input preprocessing pipelines

### 3.2 Partial and concurrent execution

* When executing a graph, users feed in data and select nodes for which they want to fetch the resulting values
* TensorFlow automatically prunes the graph so that only required nodes are evaluated
* Multiple subgraphs can run in parallel: E.g. one that reads training examples, one that preprocesses them, one that does training and one that writes parameters to disk periodically
* The same subgraph can be executed several times in parallel, e.g. that’s useful for the training part
* Concurrent executions run asynchronously, which makes it easy to implement algorithms with weak consistency requirements
* TensorFlow also has primitives to control for synchronization

### 3.3 Distributed execution

* Dataflow systems make communication explicit, which simplifies distributed execution
* Devices are assigned tasks that they execute
* Operations are implemented by different kernels for different kinds of devices. For example, matrix multiplication is an operation (~interface) and can have different kernels (implementations) for CPUs, GPUs and TPUs
* TensorFlow’s placement algorithm computes a possible set of devices for each operation, checks which operations should be colocated and then assigns devices to colocation groups
* Users can specify device preferences. This allows expert users to squeeze out even more performance
* After the placement was decided on, TensorFlow creates a subgraph for each device. Edges that cross subgraphs are replaced by *send* and *receive* nodes. The *receive* node blocks until it receives an input
* Subgraphs are cached by devices so that subsequent steps are cheaper

### 3.4 Dynamic control flow

* TensorFlow graphs support control flow in the form of conditionals (if) and iterative constructs (while)
* Several new nodes are introduced for this to work:
    * *Switch* takes data and a control input. It uses the control input  to decide where to send the data to. The outputs that do not receive the data get a *dead* value instead
    * *Merge* combines the outputs of two branches. It forwards the non-*dead* input if one exists and outputs *dead* otherwise
    * A while loop is implemented with similar operators: *Enter*, *Exit* and *NextIteration*
* Several iterations can possibly be executed at the same time
* TensorFlow also supports automatic differentiation for control flow

## 4. Extensibility Case Studies

### 4.1 Differentiation and optimization

* Gradients can be computed automatically. Given a symbolic expression, a new symbolic expression for the gradient can be derived
* Breadth-first search from the loss to the parameters is performed
* The processing of the gradient can be adapted, e.g. to implement gradient clipping or batch norm
* This also makes it easy to implement new optimization algorithms, e.g. Momentum

### 4.2 Training very large models

* When training on high-dimensional data, it is common to learn distributed representations (embeddings)
* This is commonly done when working with words as input
* Inference starts by multiplying the sparse input vector against the embedding matrix, which lets us retrieve the embeddings
* In the optimization process, only very few rows are then changed in each iteration
* To help with this, TensorFlow implements helpers for working with embeddings
* *Gather* is an operation that extracts a set of rows from a tensor
* The embedding matrix is sharded across several machines. The *Part* and *Stitch* operations help us deal with this
* Users typically do not use these operations directly but rather make use of some library helpers
* All these operations support automatic differentiation

### 4.3 Fault tolerance

* Training can take a long time and we often train on non-dedicated resources in a cluster
* These jobs can fail, so we need some sort of fault tolerance
* Implementing this on an operation level, similar to Spark’s RDDs, would be an overkill
* Again, new operations are introduced:
    * *Save* writes tensors to a checkpoint file
    * *Restore* can read these checkpoints
* Each variable is connected to a *Save* operation
* Periodically, variables are saved in new checkpoints
* If a job fails and is restarted, it attempts to restore the last checkpoint
* TensorFlow includes a library for this, where users can customize policies for checkpointing. For example, users might only want to create checkpoints for the model with the highest validation accuracy
* Synchronization guarantees are low on purpose. This makes everything cheaper and it is not too bad if a checkpoint is created a bit too early or a bit too late

### 4.4 Synchronous replica coordination

* SGD is robust to asynchronous updates
* In practice, training is also often done this way
* The TensorFlow experimented with three alternatives:
    1. Fully asynchronous updates
    2. Updates synchronized using queues. A blocking queue ensures that all workers read the same parameters. One queue per variable accumulates gradients and then applies an update atomically
    3. Just wait for the first m of n updates and apply those atomically. This makes training faster because we do not have to wait for very slow workers. MapReduce-like backup workers are started proactively

## 5. Implementation

* The core library is implemented in C++ for performance reasons
* A master node takes user requests and translates them for execution on workers
* It prunes the graph to only keep the required parts and applies optimizations, e.g. to only compute certain common subgraphs once
* The current implementation can run 10,000 subgraphs per second
* Many linear algebra parts are based on the [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page) library
* Quantization was implemented for environments where high throughput is important
* If it is too inefficient to compose many elementary functions, users can write their own kernels (operation implementations) in C++
* This was done for some performance-critical operations, such as ReLUs and sigmoids

## 6. Evaluation

### 6.1 Single-machine benchmarks

* Good performance at scale should not come at the expense of bad performance on individual machines
* Outperforms Caffe and is on the same level as Torch, since they often both use the same Kernel implementations
* [Neon](https://github.com/NervanaSystems/neon) outperforms everything by having hand-optimized kernels. TensorFlow could in principle follow this path

### 6.2 Synchronous replica microbenchmark

* The overhead for adding additional workers should be low
* When each worker has to fetch the entire model in each step, this is not the case
* However, when each worker only requires a subset of the parameters, performance improves a lot. The overhead is close to optimal here

## 7. Conclusions

* TensorFlow allows users to get good performance on their personal machines as well as in a data center
* TensorFlow as an open source software became quite popular
* Automatic optimization research could be helpful in making TensorFlow more efficient, without requiring users to fine-tune the system
