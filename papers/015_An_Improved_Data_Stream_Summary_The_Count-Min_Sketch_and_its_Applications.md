# An Improved Data Stream Summary: The Count-Min Sketch and its Applications [[pdf]](https://www.cse.unsw.edu.au/~cs9314/07s1/lectures/Lin_CS9314_References/cm-latin.pdf)

*Cormode, Graham, and Shan Muthukrishnan. "An improved data stream summary: the count-min sketch and its applications." Journal of Algorithms 55.1 (2005): 58-75.*

The Count-Min Sketch is a probabilistic data structure for approximately counting the frequencies of elements in a stream of data.
Naively maintaining a hash table with these frequencies requires a linear amount of space.
Count-Min Sketch dramatically reduces this space requirement on the cost of only giving approximate counts.
It does this by storing counts in an array that is addressed by using hashing.
This allows working with huge streams of data.

## 1. Introduction

* The *data stream scenario*: Data comes in step by step and there is not enough memory to store all of it
* We only want a summary, a sketch, of the stream
* There should be accuracy guarantees: The error should be within a factor of epsilon with probability delta
* Space and update time thus depend on these two values
* Applications in traffic analysis and in databases motivate this one-pass data stream scenario
* Sketches are generally linear functions of their input
* Typically they use a few kilobytes up to a megabyte of space
* Count-Min Sketch
    * Space used is proportional to error factor whereas it was previously quadratic
    * Only requires pairwise independent hash functions

## 2. Preliminaries

* Stream of updates (i, c): The i-th element is increased by c
* c could be any number but we are often only interested in the nonnegative case
* Point query: Count of a given element
* Range query: Sum of counts of elements in range
* Inner product query: Dot product of elements from two streams. These allow approximating the size of a join
* We can use these queries to compute more interesting statistics:
    * The items in the x-th percentile
    * Items that appear more than x% of the time

## 3. Count-Min Sketches

* Two-dimensional array with width *w* and depth *d*
* All entries are initially 0: *count[i, j] = 0*
* We set *w = e / epsilon*, *d = ln(1 / delta)*
* d pairwise independent hash functions are given
* When an update (i, c) arrives, then for each hash function *j*:
*count[j, h_j(i)] += c*

## 4. Approximate Query Answering using CM Sketches

### 4.1 Point Query

* *min_j count[j, h_j(i)]*
* Go through all hash functions, check the bucket they hash to and take the minimum
* If there were conflicts, we take the least significant one like this
* Can only overestimate

### 4.2 Inner Product Query

* Import for database query planning
* Instead of using point queries, we only take the min at the very end
* (*Note: This seems odd. It’s probably slightly faster but I wonder if not taking min earlier increases the error*)

### 4.3 Range Query

* Approximate using *dyadic ranges*

## 5. Applications of Count-Min Sketches

* Improves on best known time and space bounds for both applications

### 5.1 Quantiles in the Turnstile Model

* Problem reduces to computing range sums and doing several binary searches
* To do this, Count-Min Sketches can be used

### 5.2 Heavy Hitters in the Turnstile Model

* Again, there’s a divide and conquer procedure for finding heavy hitters
* It’s based on range sums, so Count-Min Sketches are useful

## 6. Conclusion

* Count-Min Sketch improves most error bounds from quadratic to linear in comparison to the required space
* Useful for many data stream applications
* Does not work when wanting to find correlations between elements
