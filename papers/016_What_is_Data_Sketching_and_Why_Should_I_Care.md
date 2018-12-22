# What is Data Sketching, and Why Should I Care? [[pdf]](https://pdfs.semanticscholar.org/548a/1a53896474c314ea33b9c1f96ecec8ed3b10.pdf)

*Cormode, Graham: What is Data Sketching, and Why Should I Care?*

The amount of data we store has increased quickly, much more quickly than hardware resources in the form of memory or computing power.
For some task it is thus not feasible to compute exact solutions.
Instead, the *data streaming model* can be used: A summary is computed by only looking at each element once.
After an element has been considered, it is immediately discarded.
The data streaming concept has been adapted in many domains, including search engines, social networks and finance.
This paper gives an introduction to several popular sketching algorithms.

## 1. Simply Sampling

* The simplest sketching idea: Just take a subset of the data and use that
* If we choose the elements randomly, we can estimate the standard error
    * For a sample size of *s*, the standard error is proportional to *1 / sqrt(s)*
    * Example: If we take an opinion poll with 1,000 voters, we can expect a standard error of about 3%
    * Reducing the error further becomes expensive quickly: for 0.3% we would need 100,000 voters
* To retrieve such a random sample, we could sort the elements and pick the first *s*
    * Attach a random tag to each element and sort by that tag
    * Sorting is costly
    * What do we do if new elements arrive?
* One solution is to cleverly change the probability for choosing new elements. This used to be a common interview question
* An alternative is to keep working with random tags
    * Attach a random tag to each element
    * Always keep the *s* elements with the smallest random tag
    * *(Note: It seems like this would require a heap, while the method of changing probabilities could work in O(1)?)*
* Application: Query planing in databases. Different strategies can be simulated with subsets of elements
* Shortcomings
    * Queries that require detailed knowledge of individual records cannot be answered
    * E.g. it is not possible to determine if an element is part of the stream
    * E.g. cardinality: If we see *s* different records, does that mean all records are unique or that only these *s* appear?
    * Sometimes sampling can help but other methods have a lower standard error, e.g. when estimate the frequency of elements
* Sampling is usually more flexible because we do not have to come up with the query beforehand

## 2. Summarizing Sets with Bloom Filters

* Dictionaries can answer the set membership question: Is this element stored in the data structure?
* Bloom Filters can tell us that an element is either not stored or *probably* stored
* By allowing for some uncertainty, Bloom Filters can compress the elements in very little space
* Easier problem: Answer the set membership question for integers between one and a million
    * Use a bit array
    * Set the i-th bit to 1 if the respective number is stored
    * To check if an element is stored, check whether its bit was set
    * Very compact structure: 125KB suffice
* Real data is not as nicely structured. However, we can just hash it
* We ignore hash conflicts
* To make them less likely, we hash each item several times and set several bits
* This is a trade-off: Using more hash functions makes better use of the available space, as long as enough space is available
* Commonly, we use 7 hash functions and 10 times more bits than we expect to store elements. This leads to a false positive rate of below 1%
* Best case corresponds to about half of the bits being set
* This is a modest saving when the elements are integers but a huge saving when they are strings
* Applications
    * Bloom Filters are most attractive when false positives do not introduce an error
    * For example, we perform additional work only if the Bloom Filter tells us an element is in the set. In that case false positives just lead to longer computation
    * Web browsers use it to check whether a website might be malicious. If the Bloom Filter tells us it could be, the browser checks against an external web server. Chrome and Firefox do this
    * Distributed databases, such as BigTable, Cassandra and HBase, use Bloom Filters as indexes for distributed chunks of data. This way they can efficiently keep track of which rows and columns are stored on disk

## 3. Counting with Count-Min Sketch

* To count the number of times each item appears, we can use a hash table that maps elements to counts
* If there is a huge number of different items, this requires too much memory
* The Count-Min Sketch maintains a compressed representation of these counts
* Large counts are preserved relatively accurately, while small counts incur a greater relative error
* The underlying data structure is an array of counts
* To increment a count, we hash the elements several times and increment the counts at the respective indices
* Similar to Bloom Filters but we count instead of set bits
* To retrieve a count, the elements is hashed again and the smallest respective count is returned. This leads to the name *Count-Min* sketch
* The result can be inflated
* For a sketch of size *s*, the standard error is just *1 / s*
* Similarly to Bloom Filters, Count-Min Sketches work best when overestimating the count is fine, e.g. when we only care whether an item exceeds a certain popularity
* Bloom Filters are typically proportional in size to the set. Count-Min Sketches can be much more compressed

## 4. Counting Distinct Items with HyperLogLog

* Another problem is to count the number of distinct items
* Keeping track of all elements becomes infeasible
* We could use Bloom Filters but they still require space proportional to the number of distinct elements
* HyperLogLog (HLL) expects to use space proportional to the logarithm of the logarithm of the cardinality
* This allows estimating the cardinality with an error of 1% or 2% using a couple of KB
* First idea: Use Bloom Filter and analzye the density of 0s and 1s
* To break the linearity, we can use hash functions that map to j with a probably of 2^-j
* Then, we keep track of the indices we spotted so far. This requires linear space
* HLL only stores the highest index we saw. Additionally it partitions the stream using a second hash function
* Each partition yields an estimate. These are combined using the harmonic mean
* We expect to need bits for the logarithm of the count. However, it could also require space for the count itself, if an item hashes to a large index
* By separating items into groups, the standard error is proportional to *1 / sqrt(s)*
* Popular application: Counting the number of unique visits to a page
* Facebook used it to test the *six degrees of separation claim". (The estimated degree of separation in the Facebook graph is 3.57)

## 5. Advanced Sketching

* The four examples cover most current practical applications of sketching

### 5.1 Sketching for Dimensionality Reduction

* PCA is slow because it requires computing eigenvectors of the covariance matrix
* Sketch alternative: Use a sufficient number of random projections
* Count-Min Sketches can be seen as a random projection
* Hash kernels work similarly

### 5.2 Randomized Numerical Linear Algebra

* Matrix multiplication
* Regression

### 5.3 Rich Data: Graphs and Geometry

* Capture adjacency information

## 6. Why Should I Care

* For each problem, there is a separate approach that yields an exact answer at the expense of higher memory requirements
* In many cases, the approximate method is faster and more space-efficient
* > it is sometimes claimed that Bloom Filters are one of the core technologies that “big data experts” must know
* It is important to be aware of sketching methods when working with big datasets
