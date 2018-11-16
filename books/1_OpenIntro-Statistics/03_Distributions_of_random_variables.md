## 3. Distributions of random variables

### 3.1 Normal distribution

- The *normal* (or *Gaussian*) distribution is denoted by N(μ, σ)
- N(0, 1) is the *standard normal distribution*
- The *z-score* of an observation measures how many standard deviation it is away from the mean
- *68-95-99.7 rule*:  Approximatly 68%, 95% and 99.7% of observations are within 1, 2 and 3 standard deviations of the mean in a standard normal distribution

### 3.2 Evaluating the normal approximation

- Normal probability plots can be used to see how well the normal distribution approximates another distribution

### 3.3 Geometric distribution

- A random variable with two different outcomes (success or failure, 1 or 0) is called a *Bernoulli* random variable. Probability for success is denoted by *p*
- The *geometric* distribution gives the probability that the k-th successive Bernoulli experiment is the first successful one
- It assumes that the individual experiments / random variables are *iid*: *Independent and identically distributed*. This means all variables follow the same probability distribution but don't influence each other

### 3.4 Binomial distribution

- When performing *n* iid Bernoulli experiments, the *binomial* distribution gives the probability that exactly *k* of them are successful
- Computing the binomial distribution for large values of *n* becomes very expensive because binomial coefficients grow quickly. The normal distribution gives a good approximation in this case
- However, this approximation breaks down if we are only interested in very small intervals of the distribution

### 3.5 More discrete distributions

- When performing *n* iid Bernoulli experiments, the *negative binomial* distribution gives the probability of the *k*-th success happening in the very last experiment
- For *k = 1* this is the same as the geometric distribution. Otherwise it's the same as multiplying *p* with the binomial distribution with parameters *k - 1*, *n - 1*
- The *Poisson* distribution helps estimating the number of events in discrete time steps. It's an approximation of the binomial distribution for very large *n* with a very small *p* value
