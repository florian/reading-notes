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
