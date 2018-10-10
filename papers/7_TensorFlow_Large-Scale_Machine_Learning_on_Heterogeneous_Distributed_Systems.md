# TensorFlow: Large-Scale Machine Learning on Heterogeneous Distributed Systems [[pdf]](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/45166.pdf)

*Abadi, Martín, et al. "TensorFlow: Large-scale machine learning on heterogeneous distributed systems." Preliminary White Paper 2015*

[TensorFlow](https://www.tensorflow.org) is Google's machine learning library. It lets developers use the same code to train models in various environments, e.g. on clusters or personal computers. Furthermore, it can compute gradients automatically, which makes experimenting with new kinds of models or optimizers much easier. This paper is the initial whitepaper that explains implementations of the system, describes extensions that Google built and discusses how TensorFlow was optimized.

## 1. Introduction

* When Google started exploring deep neural nets, they started out with their own library called [DistBelief](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/40565.pdf)
* After learning more about the requirements, TensorFlow was developed as the successor library
* TensorFlow takes computations described as dataflow graphs and maps them onto different hardware platforms for execution
* These platforms can be anything from mobile phones to clusters in a data center
* Having a single system for all these platforms greatly simplifies prototyping and development
* TensorFlow allows clients to express different kinds of parallelism
* TensorFlow is more flexible than DistBelief and also much more performant

## 2. Programming Model and Basic Concepts

* Computations are described using directed graphs
* Some nodes in the graph can maintain or update persistent state
* Branching and looping is possible
* Nodes represent *operations*, have zero or more inputs and zero or more outputs
* Data in the form of *tensors* flows along the edges. Tensors are arrays of arbitrary dimension
* Special edges for controlling dependencies exist. These represent *happens-before* relationships
* The implementation sometimes inserts such edges to control for memory usage

#### Operations and Kernels

* Operations have names and represent an abstract computation
* They can have attributes that must be provided or inferred during the construction of the graph (e.g. the types of the arguments)
* A *kernel* is an implementation of an operation for a specific kind of device
* For example, matrix multiplication is an operation with different implementations for CPUs and GPUs

#### Sessions

* To execute the computation described by a graph, *sessions* are used
* They allow us to run the graph in a specific environment
* To run a graph, a list of nodes that should be evaluated is required. TensorFlow then follows these nodes backwards to find all the nodes whose values need to be computed (the *transitive closure*)
* We can also pass in input values to the graph
* Most TensorFlow users have a single session that they use to iteratively evaluate the graph

#### Variables

* To maintain state between running a graph several times, *variables* can be used
* A variable is an operation that returns a handle to a persistent, mutable tensor
* The handle allows writing or reading the value that is stored
* Typically, the model parameters are stored as variables

## 3. Implementation

* The *client* is the compute node that uses the TensorFlow interface to construct a graph and send it to the *master* node
* The master then manages the execution on several *worker processes*
* There are local and distributed implementations of TensorFlow
* In the local implementation, client, master and worker all run on the same machine
* In the distributed implementation, they run on different machines. The jobs are usually managed by a cluster computing framework

#### Devices

* Each worker uses one or more devices
* The device name is composed of the device type, its index within the worker and an identifier of the task that it is running
* TensorFlow has implementations for CPUs and GPUs, but support for other device types could be added

#### Tensors

* Multi-dimensional array with a type
* References to tensors are counted. A tensor is deallocated when no references to it are left

### 3.1 Single-Device Execution

* When only one device is available, nodes are executed in an order that respects their dependencies
* A dependency count is maintained for each node
* As soon as all dependencies of a node were evaluated, it is queued for execution
* The ready queue has no specified order
* Once a node was evaluated, the dependency count of all relevant nodes is decremented

### 3.2 Multi-Device Execution

* Two main tasks
* 1. Finding out on which device a given node should be placed
* 2. Handling the communication between devices

#### 3.2.1 Node Placement

* The placement algorithm uses a cost model
* The model checks the input and output shapes of each node and uses them to compute an estimate for how long the specific operation should take
* This model is either statistically estimated using heuristics or is based on earlier measurements
* The placement algorithm then runs a simulation of the graph. It picks a device for each node using a greedy heuristic. This assignment is then also used for the actual execution
* The simulation starts with the first nodes of the graph. For each node that the simulation considers, the set of feasible devices is determined (e.g. by excluding devices that do not implement the operation)
* The device where the node would be evaluated in the quickest way is selected
* The cost model takes communication cost into account

#### 3.2.2 Cross-Device Communication

* After the placement was determined, the graph is partitioned into subgraphs, one for each device
* Edges between subgraphs are replaced with special *Send* and *Receive* nodes
* If a node would be connected to several *Receive* nodes, TensorFlow merges those *Receive* nodes. This ensures that the required tensor is only sent once for each (device, destination) pair
* By adding these two operations, the scheduling of nodes is decentralized. The master does not have to tell devices to start evaluating nodes. Instead, it just issues a single *run* command once at the beginning and afterwards everything is handled by the workers themselves. This makes the entire system more scalable

### 3.3 Distributed Execution

* Distributed execution is similar to multi-device execution
* *Send* / *Receive* nodes now use remote communication mechanisms to move the data, e.g. TCP
* Fault tolerance
    * The master periodically checks if all worker processes are still alive
    * Errors in the communication between *Send* and *Receive* are also possible
    * If such a failure is detected, the entire graph execution is aborted and restarted from scratch
    * This is not a problem because the state is stored in variables
    * Users can add checkpoints for their graphs
    * TensorFlow can use these checkpoints to automatically recover to a certain state
    * To implement this, variables are connected to *Save* nodes. These are executed periodically and write the content of the variable to disk
    * Variables are also connected to a *Restore* node that can initialize the variable when the graph is run for the first time

## 4. Extensions

### 4.1 Gradient Computation

* Many ML algorithms are based on stochastic gradient descent
* Because computing gradients is a common need, TensorFlow can do it automatically
* To compute the gradient, TensorFlow traverses backwards from a node until it finds all relevant variables. The results are then combined using the chain rule
* `tf.gradients` takes as input an operation and a list of variables. It then computes the partial derivatives of that operation with respect to the variables
* Gradient computation makes optimized execution more difficult because common heuristics break down. For example, because gradient computation reverses all steps, a lot of data needs to be stored in memory. The TensorFlow team is experimenting with various optimizations, e.g. recomputing certain values instead of keeping them in memory

### 4.2 Partial Execution

* Commonly, users only want to execute parts of the entire graph
* To implement this, users can specify what nodes they want to evaluate
* TensorFlow then traverses backwards from those nodes to find all required nodes. Only those are evaluated
* Users can also feed in data for placeholders
* TensorFlow replaces each placeholders with a special *feed* node that sends the data that was fed in
* It also connects all nodes the user was interested in to *fetch* nodes which send back their input to the client

### 4.3 Device Constraints

* Users can control on which devices TensorFlow places specific nodes
* They can specify constraints for nodes, e.g. to ensure that some nodes are placed on the same device or that other nodes are only evaluated on GPUs
* This requires changes to the placement algorithm
* A dependency graph has edges between nodes if they should be placed on the same device. TensorFlow then computes the connected components of this graph using Union-Find to determine which nodes have to be placed on the same device. Only devices that are allowed for all nodes in a component are then considered

### 4.4 Control Flow

* Adding conditionals and loops to the graph allows users to implement some things more efficiently
* The *Switch* and *Merge* operations allow implementing conditionals
* *Enter*, *Leave* and *NextIteration* can be used for loops
* Nodes inside of loops can be distributed among many devices. This makes the implementation challenging. TensorFlow rewrites the graphs to add control mechanisms to deal with this. At the beginning of an iteration, the device that owns the loop termination decision node sends a signal to all relevant devices, indicating whether the loop terminated
* Gradients can be computed even if control flow operations were used

### 4.5 Input Operations

* Inputs to the graph can be provided using feed nodes
* An alternative mechanism is directly making the graph read the input data
* This scales much better because not all of the data has to be read by the client node anymore. Sending data over the network is avoided
* Only the worker that needs the respective piece of data reads it into memory

### 4.6 Queues

* Queues allow different parts of the graph to be executed asynchronously
* The *Enqueue* operation blocks until space is a queue becomes available
* *Dequeue* blocks until a minimum number of elements are in the queue
* This could be used to prefetch data for training, for accumulating gradients before applying them, or for grouping inputs to recurrent models so that inputs of the same length can be processed more efficiently at the same time
* There’s a FIFO queue as well as a queue that just randomly selects elements (useful for randomly sampling training data)

### 4.7 Containers

* A *Container* provides backing stores for state that’s maintained between graph executions
* Variables make use of container
* Containers also allow us to share state between completely disjoint graphs that are associated with different sessions

## 5. Optimizations

### 5.1 Common Subexpression Elimination

* If graphs contain the same subgraph multiple times, TensorFlow optimizes the graph by eliminating the common subexpressions
* An algorithm by [Click](https://courses.cs.washington.edu/courses/cse501/06wi/reading/click-pldi95.pdf) is used for this

### 5.2 Controlling Data Communication and Memory Usage

* Controlling memory usage is particularly important for GPUs as they do not have that much memory
* TensorFlow performs a critical path analysis to schedule *Receive* nodes. These are specially prone to starting much earlier than necessary, so control edges are often inserted here

### 5.3 Asynchronous Kernels

* There are normal synchronous kernels as well as non-blocking kernels
* The latter ones invoke a continuation when their execution is complete
* This allows TensorFlow to avoid tying up an execution thread (e.g. when nodes are just waiting for I/O) and to make better use of the environment’s resources (e.g. memory)
* *Receive*, *Enqueue* and *Dequeue* are implemented as asynchronous kernels
* *(Note: It seems like operations that make you wait should be implemented asynchronously)*

### 5.4 Optimized Libraries for Kernel Implementations

* Some operations are implemented using highly-optimized libraries
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page) is used for many linear algebra operations
* Google patched Eigen to support tensors
* GPU libraries are extensively used

### 5.5 Lossy Compression

* Many ML algorithms are tolerant of noise and reduced arithmetic precision
* TensorFlow often uses lossy compression when sending data between devices
* 32-bit floats are converted to 16-bit floats. TensorFlow does not use the IEEE 16-bit format but the standard 32-bit format with a shorter mantissa

## 6. Status and Experience

Some insights from porting the [Inception](https://github.com/google/inception) model to TensorFlow:

1. Build tools for checking the number of parameters of a model: Automatic broadcasting makes it harder to spot mistakes
2. Start small and scale up: Port a smaller network first
3. Ensure that the loss matches between different libraries, especially when training is turned off: It is easier to spot mistakes when the model is not dynamically changing
4. Make the single machine implementation work before debugging the distributed one: This made it easier to identify race conditions
5. Guard against numerical errors: It is easy to implement training procedures that are not numerically stable
6. Analyze pieces of networks and understand the numerical error: It can be helpful to train a model using two systems and to check their different values after individual steps

## 7. Common Programming Idioms

* The following idioms help with training very large-scale models
* Data parallel training
    * If a batch size of 1,000 is used, ten batches of 100 could be computed at the same time and then combined
    * To implement this, the graph could contain 10 replicas of relevant portion of the graph
    * A single client drives the training loop and applies the combined update after each iteration
    * This can also be done asynchronously: Each worker independently applies the update as soon as they are ready
    * *(Note: The paper doesn’t make it totally clear whether users have to do this manually. It seems that it would make more sense to dynamically rewrite the graph depending on how many machines are available)*
* Model parallel training: Several parts of a model can be evaluated in parallel
* Concurrent steps for model computation pipelining: One can also run several steps concurrently on the same device to make sure that it really is fully utilized

## 8. Performance

This section was moved to the [2016 paper](papers/6_TensorFlow_A_system_for_large-scale_machine_learning.md)

## 9.  Tools

### 9.1 TensorBoard

* TensorBoard is a GUI that helps with understanding the constructed graph and the behavior of the trained ML models
* Graph
    * Many networks have a lot of nodes in TensorFlow graphs (Inception has 36k, an LSTM for language modeling has 15k)
    * Naive visualization techniques do not scale well enough for these use cases. TensorBoard instead collapses nodes to help users see the general structure
    * Groups with identical structure are highlighted
    * High-degree nodes are displayed in a separate part of the screen to reduce visual clutter
    * The entire interaction is interactive
* Summary data
    * During training, users generally want to see information about their model
    * Scalar summaries are displayed over time, e.g. the loss
    * Histogram summaries (e.g. the distribution of weights) and image-based summaries (e.g. visualizations of layers) are also supported
    * This is implemented using summary nodes that are executed every few iterations
    * The client driver then writes the summary data to a log which is periodically read by TensorBoard

### 9.2 Performance Tracing

* To analyze performance data, Google developed a tool just for TensorFlow
* This tool collects a lot of performance related data and visualizes it

## 10. Future Work

* Idea: Make it easier to reuse subgraphs. These building blocks could be written in one language and then used in another
* This would make it easy for researchers to publish reusable components
* To improve performance, a JIT compiler could take a subgraph and optimize it based on runtime profiling information
* Currently there are heuristics in place for node placement and scheduling. These could be replaced with learned systems

## 11. Related Work

* Theano, Torch, Caffe and Chainer all map the computation to a single device
* Theano and Chainer also support symbolic differentiation
* Caffe has its core written in C++, which makes deployment easy
* Like DistBelief and Project Adam, TensorFlow can spread out computations among multiple devices. However, TensorFlow’s system is much more flexible than parameter servers because weight updating is built right into the graph
* [Halide](http://halide-lang.org/) uses an internal representation similar to TensorFlow’s dataflow graph. However, it has much deeper knowledge about the operations and can thus optimize much more. TensorFlow could be extended with such optimization mechanisms
* Flume also uses dataflow graphs
* Spark is optimized for computations that access the same intermediate result several times. Resilient distributed datasets are cached outputs of earlier computations

## 12. Conclusions

* TensorFlow offers a flexible dataflow-based programming model
* There are single machine and distributed implementations
