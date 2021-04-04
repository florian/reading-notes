## 5. Inference for numerical data

### 5.1 One-sample means with the t-distribution

- The normal distribution is a bad approximation if the sample size is too small. The *t-distribution* tries to fix this by taking the sample size into account
- The t-distribution is parameterized by *degrees of freedom*. This parameter is set to *n - 1* where *n* is the number of data points in the sample
- The t-distribution converges to the normal distribution with increasing degrees of freedom. For small degrees of freedom, it gives a better approximation than the normal distribution
- The other two constraints still need to hold when using the t-distribution:
  - The observations need to be independent
  - The sample distribution needs to be approximately normal
- Otherwise, tests based on the t-distribution (*t-tests*) work exactly as tests using the normal distribution
- For t-tests, the z-score is called *t-score* instead

### 5.2 Paired data

- Paired data means we have samples from two distributions where observations from one sample can be mapped bijectively to the other sample
- If we are interested in the mean, we can just compute the difference in means between the two samples and use this in the same way as before

### 5.3 Difference of two means

- If the two samples are not paired, then the standard error needs to be computed differently
- *SE = sqrt(var1 / n1 + var2 / n2)*. This follows out of computing the standard deviation of *X - Y* where *X, Y* are random variables
- The degrees of freedom also need to be chosen differently here: *df = min(n1 - 1, n2 - 1)*
- If the standard deviations of both samples are nearly the same (and there are ways to justify why this is the case), he *pooled standard deviation* is a better estimate of the standard error

### 5.4 Power calculations for a difference of means

- When performing studies, we want to make sure that we detect an effect if it is real and has practical significance
- The probability for this happening is called the *power*. It can be computed for different *effect sizes* and sample sizes
- By performing a test, we get a distribution for the null hypothesis. We can get another distribution if we center the same distribution at the effect size. This distribution shows us possible results given that the exact effect size is the true mean
- The power is the inverse probability of the area in which the effect distribution overlaps with the area where the null hypothesis is accepted
- Optimally, the two distributions overlap as little as possible. A larger sample size means that the standard error is smaller and the two distributions have less overlap
- Choosing an appropriate sample size is a trade-off. We need it to be large enough so that the power is acceptable. On the other hand, it should not be too large as we don't want too many people in the treatment group (for financial or medical/ethical reasons)
- Given a specific power level that we want to achieve, we can solve the equation for *n* and thus find the optimal sample size for our study

### 5.5 Comparing many means with ANOVA

- Sometimes we want to compare the means of *k* groups. Comparing all pairs of means individually doesn't really work because this number grows quadratically and at some point we will just find a difference by chance
- *ANOVA* (analysis of variance) provides a solution for this, by comparing the variability inside groups with the total variability. *H0* is that all means are the same
- The *mean squared error between groups MSG* measures the total variability
- The *mean squared error MSE* measures the variability inside the individual groups
- When *H0* is true, *MSG* and *MSE* should be about equal, so F = *MSG / MSE* should be close to *1*
- The *F-distribution* is a probability distribution over the *F* statistic. It's parameterized by the degrees of freedom of *MSG* and *MSE*
- This distribution can then be used for a one-sided *F-test*
- This test requires the individual variances of all groups to be approximately equal. As usual, we also need independence and a nearly normal distribution
- When *H0* is rejected, we might be interested in knowing which means are different. This can be tested by performing many individual tests between pairs of means. In this case, a more strict significance level should be used
- The *Bonferroni corrected* significance level is *α / K* where *K* is the number of tests performed
