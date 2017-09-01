# [OpenIntro Statistics](https://www.openintro.org/stat)

## 1. Introduction to data

### 1.1 Case study: using stents to prevent strokes

When performing experiments, patients are partitioned into two groups:
- *treatment*: Patients on which the experiment is actually performed
- *control*: Patients where the experiment is not performed but which are otherwise handled in the same way

### 1.2 Data basics

Variables can have different types. Generally, they can be classified into two groups which have their own subgroups:
- *Numerical* (or *quantitative*): The values of the feature are numeric and it makes sense to perform arithmetic operations on them (e.g. addition, subtraction, calculating the mean)
  - *Discrete*: There are no jumps between the values (e.g. an integer that describes the age of a person)
  - *Continuous*: There are jumps between the values (e.g. a number describing the price someone paid for a house)
- *Categorical* (or *qualitative*): Variables that have a finite set of possible values
  - *Ordinal*: There is sensible way to order the values (e.g. there are three different price classes)
  - *Nominal*: There is no sensible way to compare or order the values (e.g. colors). Zip codes are another example of a nominal variable. Even though they are numbers (in some countries at least), it does not make sense to perform arithmetic operations on them and they have no sensible ordering
  - *Binomial*: There are only two different values (e.g. currently married or not)

There can be relations between pairs of variables:
- If one variable allows us to make conclusions about the value of another variable, we call them *associated* or *dependent* variables
- If not, they are *independent*
- If an increased value for one variable generally also means an increased value for the other variable, they are *positively associated*. If both generally decrease together, they are *negatively associated*
- *Correlation* is a special case of association where the relationship is linear
- Neither association nor correlation imply causation

### 1.3 Overview of data collection principles

- Every question we want to answer is based on a target *population*
- Often, we can't collect data about the entire population, so we work on a *sample* of the population
- Anecdotes generally don't have much value because they are usually based on few data points and humans tend to remember outliers much more
- Sampling can be difficult because it's easy to select a subpopulation that's not representative
- If one variable *a* seems to affect another variable *b*, we call *a* the *explanatory variable* and *b* the *response variable*. This does guarantee a causal relation, it just means we suspect one
- A *cohort* is a group of patients that have a similar characteristic

### 1.4 Observational studies and sampling strategies

Sometimes two variables seem associated but are actually both caused by a third variable. This third variable is called a *confounding variable*.

There are different ways of sampling from the population:
1. *Simple random sampling*: Individuals are uniformly, randomly selected
2. *Stratified sampling*: The population is partitioned into groups called *strata* so that similar individuals are grouped together. Then, a few individuals are sampled (using simple random sampling) from each strata. This helps make the sample more representative of the entire population
3. *Cluster sampling*: The population is partitioned into groups called *clusters* so that the clusters are all similar to each other and the variability inside each cluster is similar to the one of the entire population. Then, one or more clusters are randomly chosen and their entire population is selected for the sample. This makes it more economical to choose good samples
4. *Multistage cluster sampling*: From each cluster, only a few individuals are randomly selected. Depending on the size of the clusters, this might be a better alternative

### 1.5 Experiments

Experiments are based on at least four principles:
1. *Controlling*: Researchers should try to control for differences in the groups as well as possible; e.g. when testing a pill, all patients might be required to drink a certain amount of water to control for the fact that otherwise they would take the pill with different amounts of water
2. *Randomization*: Because of variables that can't be controlled for, patients should be assigned to treatment and control randomly
3. *Replication*: In an individual study, we can try to replicate by using a larger sample. Entire studies can be replicated to validate earlier findings
4. *Blocking*: If it seems like a variable other than the treatment influences the result, a technique called blocking can be applied. Here, the patients are assigned to treatment and control so that the distribution of that variable is nearly the same in both groups

Generally, patients should not know whether they are in treatment or control. If this is the case, the study is called *blind*. Additionally, the people testing the patients should also not know. This is called *double-blind*.

### 1.6 Examining numerical data

- Scatter plots are helpful for visualizing the relationship between two variables
- Histograms provide an overview over the density of different values
- When the data slowly trails off in one direction, it is said to have a *long tail* in that direction
- When the data has a long tail to the left (right), it is *left skewed* (*right skewed*). When it is both left and right skewed, it is said to be *symmetric*
- The *mode* is a prominent peak in the data. Sometimes, more formally, it is considered to be the most common value. Depending on the number of modes, the distribution is called *unimodal* (1), *bimodal* (2) or *multimodal* (3 or more)
- *Variance* is the average squared distance to the mean. Squaring penalizes large deviations from the mean
- The *standard deviation* is the square root of the variance. Its value is given in the same unit as the mean. Both variance and standard deviation are measures of variability in the data
- Mean, variance and standard deviation are highly affected by outliers. The *median* and *interquartile range* (*ICR = Q3 - Q1*) are much more robust against outliers
- *Outliers* are observations that seem extreme compared to the rest of the data
- To work with some variables more easily, it can be helpful to transform the values, e.g. putting them on a logarithmic scale might make it reasonable to fit a linear model
- Depending on the data, more specialized visualizations can be great, e.g. using heatmaps for geographical data

### 1.7 Considering categorical data

- *Contingency tables* provide a basic summary of the relationship between two variables
- There are various graphical visualizations for contigency tables, e.g. *segmented bar plots* or *mosaic plots*
- *Pie charts* should be avoided because they make it hard to compare frequencies

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

## 7. Introduction to linear regression

### 7.1 Line fitting, residuals, and correlation

- We want to estimate the value of one variable by using a linear function of another variable
- The estimated variable is called the *predictor variable*, the other the *explanatory variable*
- A hat ^ on top of a variable name signifies that it's an estimate
- After having fitted a linear model, we can compute the difference between the actual value and the estimated value of each data point. This difference is called the *residual*
- The actual values can be described using the estimate and the error: *y = y_hat + residuals*
- Plotting the residuals can be helpful to check if a linear model is suitable
- *Correlation* is a measure for the strength of a linear relationship. It's a value between -1 and 1, where -1 is called a *negative correlation*, 0 means no correlation at all, and 1 is called a *positive correlation*. Correlation is denoted by *R*

### 7.2 Fitting a line by least squares regression

- There are many ways to fit a line. The most popular way is the *least squares criterion*, i.e. to choose the line that minimizes the sum of squared residuals
- Alternatively, one could try to minimize the sum of absolute residuals. This is a bit more complicated and it should only be done if there's is a good reason
- Conditions for least squares:
	- Linearity: There needs to be a good way to fit a linear model at all
	- Nearly normally distributed residuals: Might not be the case when specific outliers occur
	- Constant variability: As we move across the x-axis, the variability of residuals should stay similar
	- Independent observations: Is generally not the case for time series data
- *Extrapolation* is difficult, i.e. it's hard to make estimates for input data that's completely different to what we've seen before
- *R-squared R^2* describes the quality of a fit: It's a value between 0 and 1 that quantifies how much variation is explained by the model. *R^2 = 0.9* means our model explains 90% of the variation of the predictor variable
- Categorical values can be used for regression by converting them to numerical values

### 7.3 Types of outliers in linear regression

- Linear regression is susceptible to outliers
- Data points that are horizontally far away from the center of the data are called data points with *high leverage*
- If these data points have a large effect on the fitted line, they're called *influential points*
- Outliers should only be removed for fitting a model if there's a good reason to do so. They often help to fit a model that generalizes better

### 7.4 Inference for linear regression

- After having fitted a model, we're interested in finding out whether we just matched noise, or if there's sufficient evidence for the chosen coefficients
- *H0*: The true linear model has slope 0, i..e all coefficients are 0
- Standard error can be computed and using that, a t-test can be performed
- Depending on the exact use-case, a one-sided or two-sided test should be used

## 8. Multiple and logistic regression

### 8.1 Introduction to multiple regression

- By using a multivariate linear regression, we can control for many variables at the same time
- *Adjusted R^2* is a better tool for model assessment when there are multiple variables

### 8.2 Model selection

- A model that uses all available variables is called a *full model*
- Often, we don't want to use this model because it might be too complicated to generalize well
- For variable selection, there are at least two simple approaches:
	- *Backward elimination*: Start by using all variables, calculate the adjusted R^2. Then test if removing any single variable would improve this metric. If so, remove the variable that leads to the best increase and repeat
	- *Forward selection*: Start by using the most useful variable and then keep on adding the next most useful variable until no new variable improves the adjusted R^2
- Alternatively, the p-value can be used instead of the adjusted R^2. Here, we only add a variable if it has a p-value above the significance level. Alternatively, we can remove variables as long as we find variables with a p-value below the significance level

### 8.3 Checking model assumptions using graphs

- We can check the four conditions using plots:
	- The residuals should be normally distributed. We can check this using a normal probability plot
	- The variability of residuals should be constant: Scatter plot of the absolute residuals and fitted values, there should be no pattern
	- The residuals are independent of each other: Plot residuals in the order of the data collection, check that there's no pattern
	- Each variable is linearly related with the outcome: For each variable, create a scatter plot of the variable and the residuals, check if a line would fit the pattern well
- Confidence intervals and p-values work the same way in multivariate logistic regression as they do in simple linear regression
- No model is perfect. Even imperfect models can be useful if there shortcomings are properly communicated

### 8.4 Introduction to logistic regression

- In *logistic regression*, we want to predict the outcome of a categorical variable (0 or 1)
- To do this, we perform a linear regression and use the logistic function to map the outcome between 0 and 1
- This mapped value can be interpreted as a probability. We can then decide where to set a threshold for reporting 0, 1 or reporting that we're not confident in any result
- When estimating the probability for label 1,  the portion of data points where we estimate a 10% probability should only be 10% large. We can check if this is the case by using *natural splines* 
- To create a great model, it can be useful to hand-engineer useful variables
