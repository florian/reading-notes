## 4. Foundations for inference

### 4.1 Variability in estimates

- A *point estimate* is an estimate of a statistical value using sample data
- Depending on the sample that is chosen, this estimate is different. If we compute the point estimate for all possible sample subsets, we get a *sampling distribution* of the estimate
- The standard deviation of this distribution is called the *standard error* (*SE*) of the estimate. It describes the uncertainty that we have about an estimate
- If we are interested in estimating a mean, we can estimate the standard error using *SE = σ / sqrt(n)* where *σ* is the standard deviation of the sample and *n* is the number of sampled data points

### 4.2 Confidence intervals

- A *confidence interval* is a plausible range of values for the point estimate
- *CI = point estimate ± z \* SE* where *z* corresponds to the selected confidence level, e.g. 1.96 for a 95% confidence level. If an estimate is outside this interval, we can be 95% confident that it wasn't just by pure chance
- Conditions for approximating the sampling distribution well with a normal distribution:
  - The individual observations need to be independent of each other. We can assume that this is the case if the sample consists of fewer than 10% of the population
  - We need enough data points (*n > 30*)
  - The population distribution may not be skewed too strongly
- *z \* SE* is called the *margin of error*
- Confidence is something different than probability, it just quantifies plausibility

### 4.3 Hypothesis testing

- The *null hypothesis* H0 represents a skeptical viewpoint, i.e. it's generally the assumption that nothing changed
- The *alternative hypothesis* HA represents the opposite viewpoint
- H0 is only rejected if there is sufficient evidence in favor of HA. Otherwise, we say that we failed to reject H0
- The value of the parameter if H0 is true is called the *null value*
- A *type 1 error* occured if we incorrectly rejected H0
- A *type 2 error* occured if we incorrectly failed to reject H0
- If *positive* means that H0 was rejected, then a type 1 error corresponds to a *false positive* and a type 2 error corresponds to a *false negative*
- The *significance level α* quantifies how strong the evidence needs to be to reject H0. A value of *α = 0.05* means that we only reject H0 if the estimate is outside a 95% confidence interval. This quantifies how often we are willing to make a type 1 error
- The *p-value* is the probability that we make an observation that is at least as favorable to HA as our current observation, given the assumption that H0 is true. This quantifies how much evidence there is for H0 to be true
- Depending on the exact question we want to answer, a *one-sided* or *two-sided* test should be used
- H0 is rejected if the p-value is smaller than the significance level. It is important to choose the significance level beforehand
- Finding a good significance level is a trade-off between how much we are willing to make type 1 and type 2 errors. A smaller significance level, means we make fewer type 1 errors but more type 2 errors. Depending on the problem, one of these errors might be much worse than the other kind
- *null distribution*: sampling distribution given that H0 is true

### 4.4 Examining the Central Limit Theorem

- The *central limit theorem* states that when independent random variables are summed up, the resulting distribution is normal in most cases. The more random variables we add up, the closer we get to a normal distribution
- This theorem is the reason why it often makes sense to use significance tests based on the normal distribution

### 4.5 Inference for other estimators

- A point estimate is *unbiased* if it's the center of the sampling distribution
- Significance in statistics means something different than it does in general language, i.e. statistical significance does not imply practical significance, it just means we are confident that the finding is not by chance
