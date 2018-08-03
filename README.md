---
title: "Week7"
output:
  word_document: default
  pdf_document: default
  html_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Now we are moving onto correlation. Pearson's Correlation (correlation between two continuous variables) measures the relationship between two variables.  Correlation is measured on a negative one to positive one scale. Correlations that are negative means that as one variable increases the other decreases.  Correlations that are positive means that as one variable increases so does the other.  Larger values on the negative or positive side indicate a stronger relationship.  We can also get a p-value; however, beware that large samples make everything seem significant.    

FYI: Correlation is the standardized form of covariance.  So covariance is not on a negative one to positive one scale.

Let's load the data
```{r, echo=FALSE}
set.seed(123)
fideility = rnorm(100, 50, 2)
satSample = c(1:5)
satisfaction = sample(satSample, 100, replace = TRUE)
PHQ9Pre = round(rnorm(100, 50, 4),0)
PHQ9Post = round(rnorm(100, 40, 4), 0)
GAD7Pre = round(rnorm(100, 50, 4),0)
GAD7Post = round(rnorm(100, 40, 4), 0)
genderSamp = c(1,0)
gender = as.factor(sample(genderSamp, 100, prob = c(.5, .5), replace = TRUE))
treatment = as.factor(sample(genderSamp, 100, prob = c(.5, .5), replace = TRUE))
```
Now let us look at the correlation between the PHQ9 pre scores and the GAD7 pre scores. (I apologize I was lazy and didn't make these correlated) 

cor.test adds the p-value

cor is useful for multiple correlations; however, you need to put the variables into a data frame first.

Remember for a correlation matrix values above and below the ones are the same.  There are ones because a variable is always perfectly correlated with itself. 
```{r}
library(psych)
PHQ9_GAD7Week7 = data.frame(PHQ9Pre, GAD7Pre, PHQ9Post)

corr.test(PHQ9_GAD7Week7)

cor.test(PHQ9Pre, GAD7Pre)

dat = data.frame(PHQ9Pre, GAD7Pre, PHQ9Post, GAD7Post)
cor(dat)
```
For two categorical variables, we can use a chi-square test to evaluate whether being in one category is related or dependent upon being in another category.  For example, if we randomized people into an intervention and control group we would expect there not to be a relationship between assignment to treatment and gender.  So let us test this assumption
```{r}
chisq.test(gender, treatment)
```
To evaluate if there is a relationship between a continuous variable and a categorical variable you could use an ANOVA.  Be careful with ANOVAs, as it makes many assumptions (e.g. equal variances among groups, equal groups size).  ANOVA doesn't work with repeated measures and unequal sample sizes, which is almost always the case.

Below we are looking at if there is a difference between treatment assignment and a GAD7 difference score.  You want to save the results as an object then run the summary function to get the F-Value and p-value. 
```{r}
anovaDat = data.frame(GAD7Diff = GAD7Post-GAD7Pre, treatment) 
anovaResults= aov(GAD7Diff ~ treatment, data =  anovaDat)
summary(anovaResults)
```
For the example, above we could also run a regression.  

Intercept: Generally it is the predicted or expected mean of y when all other covariates (e.g. coefficient, regressors, independent variables) are zero.  In this example, it is the predicted mean GAD7Diff score for the treatment group.  In this case the expected difference score for those not in the treatment is about -9.  We usually don't interpret it, it just needs to be in there for the math to work.  

y = b0 + b1*(treatment) + e

y is the predicted value of GAD7Diff, which is based on the intercept (expected mean of GAD7Diff when treatment is zero). Beta1 is the slope coefficient for the treatment variable.  The interpretation is that relative to those in the control group, for those in the treatment, there is a -.39 decrease in GAD7 difference scores.  
```{r}
regressionDat = data.frame(GAD7PDiff = GAD7Post-GAD7Pre, treatment)
regressionResults = lm(GAD7PDiff ~ treatment, data = regressionDat)
summary(regressionResults)
corr.test(GAD7PDiff, GAD7Pre)

```
Next week we will talk about the assumptions of linear regression, more intrepretations, R^2 squared, and other diagnostis for evaluating model fit.  
