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
