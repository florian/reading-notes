## 6. Inference for categorical data

### 6.1 Inference for a single proportion

- When working with categorical variables, the *proportion* of times a certain result appears in a sample is often interesting
- This can be described as a mean of `1`s (for the respective value) and `0`s  (for anything else)
- The sample proportion is approximately normally distributed if:
	- The individual observations are independent
	- *success-failure condition*: We expect to see at least 10 successes and 10 failures, where a failure is anything other than the value we're interested in
- *SE = sqrt(p \* (1 - p) / n)*
- The true *p* is unknown, so we estimate it using the sample data: The proportion of times the value we're interested in was observed
- Confidence intervals and p-values can then be computed as usually
- For confidence intervals, we're often interested in keeping the margin of error (*z \* SE*) below some value, e.g. 0.04
- We want to find the smallest sample size, so that the upper bound for the margin of error holds even in the worst case. This is done by substituting *p* with its worst-case value and then solving for *n*

### 6.2 Difference of two proportions

- The difference of two proportions *p1 - p2* is approximately normal if:
	- Both proportions follow the normal model
	- The two samples are independent of each other
- *SE = sqrt(SE1^2 + SE2^2)* where *SE1, SE2* are the individual standard errors
- Pooled proportion estimate should be used when *p1 = p2* according to *H0*: p_estimate = number of successes / number of cases, taking both samples into account

### 6.3 Testing for goodness of fit using chi-square

- Two primary applications:
	- Given observations that can be classified into groups, determine if the sample is representative of a population
	- Check how likely it is that the sample is from a given distribution
- *chi-squared*: For each class, compute the squared error (*observed proportion - expected*) and divide it by the standard error, sum all of these results up
- *SE = sqrt(n)* where *n* is the number of observations for a specific class
- The *chi-squared* distribution is parametrized by degrees of freedom: *number of classes - 1*
- p-value can be computed by calculating the chi-square statistic and then checking the probability for a result at least as extreme in the respective chi-squared distribution
- Conditions for using chi-square:
	- Independence of all observations
	- Each group must have at least five expected observations
- It's a one-sided test because a large value implies an unlikely result but a small value means *H0* is very likely to be true

### 6.4 Testing for independence in two-way tables

- In a *two-way table*, the outcomes for combinations of two variables are shown
- The inner cells contain *joint frequencies*, the row / column totals describe *marginal frequencies*
- We want to test whether the individual variables are independent of each other
- *H0*: Variables are independent. The expected frequencies can then be found by computing the average frequencies and multiplying them with the observation size for the respective value, to get expected counts
- These expectations can be used to calculate the chi-square statistics
- For two-way tests with chi-square, the degrees of freedom are *df = (R - 1) \* (C - 1)* where *R, C* are the number of rows, columns. In other words, *R, C* describe the number of values/bins that we consider for the two variables

### 6.5 Small sample hypothesis testing for a proportion

- When the success-failure condition is not met, we can't use the large sample hypothesis framework discussed so far, since it requires a good approximation of the normal distribution
- Instead, we can run a computer simulation to estimate the sample distribution more closely, by assuming that *H0* is true
- The p-value can then be computed using this distribution
- Because the distribution is approximate, when we double the p-value in a two-sided test, we might get a value larger than *1*. In that case, the p-value is said to be 1
- Depending on the exact *H0*, its conditions can be used to create an exact sample distribution (again assuming H0 is true), e.g. by using the binomial distribution

### 6.6 Randomization test

- When investigating in the difference between two proportions, a *permutation test* is one kind of small sample hypothesis test
- If *H0* is true, this means that all differences were purely by chance
- We can randomly reassign patients to both groups, and check how that changes the difference in proportions
- By doing this for many permutations, we get a distribution over the difference in proportions, given that *H0* is true
- This distribution can then be used to compute a p-value
