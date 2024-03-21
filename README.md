# power-outage-attacks
by Matilda Gaddi


---

## Introduction
Power outages have the ability to impact the wellbeing of many people. Major power outages occur every year, so it is important to analyze and try to predict causes and characteristics of power outages. This project will use the dataset described below:

_The major outages witnessed by different states in the continental U.S. during January 2000–July 2016. As defined by the Department of Energy, the major outages refer to those that impacted atleast 50,000 customers or caused an unplanned firm load loss of atleast 300 MW. Besides major outage data, this article also presents data on geographical location of the outages, date and time of the outages, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages. This dataset can be used to identify and analyze the historical trends and patterns of the major outages and identify and assess the risk predictors associated with sustained power outages in the continental U.S._<br>(Mukherjee et al.)<br>

_This data contains information on geographical location of the outages, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages._<br>(Purdue University)<br>

This dataset is accessible at https://engineering.purdue.edu/LASCI/research-data/outages<br>
with detailed information at https://www.sciencedirect.com/science/article/pii/S2352340918307182

### Guiding Question

### Dataset Description
observations: 1534
revelant columns: 
- CAUSE.CATEGORY
- 
-
-



---

## Cleaning and EDA
### Cleaned DataFrame
The dataframe was loaded from an excel file using pandas' read_excel. Redudant and irrelevant columns and rows were dropped, for example, the observation number (1, 2, 3 ...).
(head)

### Univariate Analysis
### Bivariate Anaylsis
### Pivot Table

---

## Assessment of Missingness

### NMAR Analysis
DEMAND.LOSS.MW could be Not Missing At Random (NMAR) with the reasoning that if demand loss is very low, it is less likely to be reported and present in the dataset.

### Missingness Dependency (MAR Analysis)
CUSTOMERS.AFFECTED has 443 missing values (29% of observations). This seems like an important piece of information so we would like to investigate whether it's missingness is dependent on other columns in this dataset. 

We ran two permutation tests on #1 OUTAGE.DURATION and #2 RES.SALES as possible factors the CUSTOMERS.AFFECTED missingness could be dependent on.

#1 Our test statistic was the difference in the means of OUTAGE.DURATION when CUSTOMERS.AFFECTED was missing and when it was not missing.

[insert graph]

We chose a significance level of 0.05. We reject the null hypothesis that CUSTOMERS.AFFECTED is not dependent on OUTAGE.DURATION with a p-value of 0.0062.

#2 Our test statistic was the difference in the means of RES.SALES when CUSTOMERS.AFFECTED was missing and when it was not missing.

[insert graph]

We chose a significance level of 0.05. We fail to reject the null hypothesis that CUSTOMERS.AFFECTED is not dependent on RES.SALES with a p-value of 0.1298.

### Missing Value Imputation
DEMAND.LOSS.MW had around 200 values at 0 already. It's missing values were imputed with 0 assuming that they are missing because the true values are an insignificant amount.


---

## Hypothesis Testing

Null Hypothesis: the mean duration of power outages caused by severe weather is equal compared to other causes combined. (mean severe - mean not-severe = 0)

Alternate Hypothesis: the mean duration of power outages caused by severe weather is greater than other causes combined. (mean severe - mean not-severe > 0)

The test statistic we are using is the difference in means between severe-weather outage duration and not-severe-weather outage duration.


We chose a significance level of 0.05 and the p-value is 0.0 so we reject the null hypthesis in favor of the alternative. 


---

## Framing a Prediction Problem
We'd like to predict whether an outage was caused my an intentional attack. We are using a random forest classifier for binary classifiation of the CAUSE.CATEGORY variable.
We are using the F1 score as our metric because of the inbalance in classes (there are many more non-attack outages than attack outages). 

---

## Baseline Model
Since 73% of the outages are not caused by attacks, we want our baseline model to perform better than a constant false prediction, so better than 73% accuracy.
For the models features we used NERC.REGION which is nominal so we one-hot-encoded it, and we used CLIMATE.REGION also nominal and also one-hot-encoded.

The accuracy of this model is 75.26%
However the F1 score of this model is 0.492 which has lots of room to improve.

This performance is not as bad as a constant prediction, but is not good because it doesn't improve much on it and has a lot more room to be more accurate and have a higher F1 score.

---

## Final Model
To improve on our baseline we added features 
We added these because

We used gridsearch to finder optimal hyperparameters of 16 for the maximum depth of the decision trees, and 2 for the minumum sample split.

The accuracy of this model is 87.76%, but more importantly it's F1 score is 0.791 which are both great improvements from the baseline.

---

## Fairness Testing
We tested whether the model performs fairly on outages that occured in places with low vs high population (above and below 8769252 inhabitants, the median population in our dataset). Our evaluation metric was accuracy {{{[[[F1?]]]}}}.

Null Hypothesis: The model has the same accuracy on cases with low populations and high populations.

Alternative Hypothesis: The model has different accuracy on cases with low populations and high populations.

The test statistic we used for a permutation test was the absolute value of low population accuracy - high population accuracy.

We chose a significance level of 0.05 and the p-value is 0.553, so we fail to reject the null hypothesis. We can be more confident that our model performs similarly on these two groups.


---