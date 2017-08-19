# OpenIntro Statistics

[OpenIntro Statistics](https://www.openintro.org/stat/) is a freely available textbook that's meant to introduce readers to statistics.

## 1. Introduction to data

### 1.1 Case study: using stents to prevent strokes

When performing experiments, patients are partitioned into two groups:
- *treatment*: Patients on which the experiment is actually performed
- *control*: Patients where the experiment is not performed but which are otherwise handled in the same way

### 1.2 Data basics

Variables can have different types. Generally, they can be classified into two groups which have their own subgroups:
- *Numerical* (or *quantitative*): The values of the feature are numeric and it makes sense to perform arithmetic operations on them (e.g. addition, subtraction, calculating the mean)
- - *Discrete*: There are no jumps between the values (e.g. an integer that describes the age of a person)
- - *Continuous*: There are jumps between the values (e.g. a number describing the price someone paid for a house)
- *Categorical* (or *qualitative*): Variables that have a finite set of possible values
- - *Ordinal*: There is sensible way to order the values (e.g. there are three different price classes)
- - *Nominal*: There is no sensible way to compare or order the values (e.g. colors). Zip codes are another example of a nominal variable. Even though they are numbers (in some countries at least), it does not make sense to perform arithmetic operations on them and they have no sensible ordering
- - *Binomial*: There are only two different values (e.g. currently married or not)

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
