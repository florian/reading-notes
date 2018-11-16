## 2. Probability

Note: Prior to reading this book I knew much more about probability theory than about statistics, so the summary of this chapter is very brief.

### 2.1 Defining probability

- Law of large numbers: As we sample more and more elements, the proportion each element was observed converges to its probability
- A *distribution* is a function that assigns probabilities to events. In the finite case, it might just be a lookup table
- Two events are *independent* if knowing the outcome of one event does not change the probability of the second event

### 2.2 Conditional probability

- A *marginal* probability is the probability of a single event occuring (also called *unconditional* probability): *P(A)*
- The probability of one or more events occuring at the same time is called *joint* probability and can be denoted by *P(A, B*) as well as *P(A and B)*
- A *conditional* probability is the probability for event *A* given that we already know a second event *B* occured: *P(A|B)*
- *Tree diagrams* show marginal probabilities in the primary branches, conditional probabilities in all other branches, and joint probabilities in the leafs
- *Bayesian statistics* is a branch of statistics that's based on using *Bayes' rules* to update our beliefs when new information is available

### 2.3 Sampling from a small population

- Sampling with replacement implies our observations are independent of each other, which is a very nice property to have
- For sampling without replacement this is not the case. However, when we sample from a lot of elements the difference can be negligible
- Sampling with replacement means that the individual results are less likely but that they can occur more than once

### 2.4 Random variables

- The expected value of a linear combination of random variables is equal to the linear combination of the individual expected values using the same coefficients
- For variance this also holds if the random variables are independent of each other and the coefficients in the linear combination for the combined variance are squared
