# Attack Power Outage Investigation and Prediction
by Matilda Gaddi


---

## Introduction
Power outages have the ability to impact the wellbeing of many people. Major power outages occur every year, so it is important to analyze and try to predict causes and characteristics of power outages. This project will use the dataset described below:

_The major outages witnessed by different states in the continental U.S. during January 2000–July 2016. As defined by the Department of Energy, the major outages refer to those that impacted atleast 50,000 customers or caused an unplanned firm load loss of at least 300 MW. Besides major outage data, this article also presents data on geographical location of the outages, date and time of the outages, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages. This dataset can be used to identify and analyze the historical trends and patterns of the major outages and identify and assess the risk predictors associated with sustained power outages in the continental U.S._<br>(Mukherjee et al.)<br>

_This data contains information on geographical location of the outages, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages._<br>(Purdue University)<br>

This dataset is accessible at https://engineering.purdue.edu/LASCI/research-data/outages<br>
with detailed information at https://www.sciencedirect.com/science/article/pii/S2352340918307182

### Guiding Question
This project will be exploring "How can we predict if a power outage was caused by an intentional attack?" and related questions about the causes of outages, their durations, and other factors.

### Dataset Description
observations: 1534

revelant columns: 
- CAUSE.CATEGORY : the cause of the outage (severe weather, intentional attack, etc.)
- U.S.\_STATE : the name of the state the outage occured in
- CLIMATE.REGION : the region the outage occured in (Northeast, West, etc)
- NERC.REGION : the North American Electric Reliability Corportation region involved in the outage
- TOTAL.CUSTOMERS : the number of customers in the state
- TOTAL.PRICE : the average monthly electricity price in the state (cents/kilowatt-hour)
- TOTAL.SALES : the total electricity consumption in the state (megawatt-hour)
- ANOMALY.LEVEL : a numerical representation of the El Niño/La Niña oceanic climate event
- TOTAL.REALGSP : total real gross state product of the state 
- RES.CUST.PCT : the percent of customers of the state in the residential sector
- COM.CUST.PCT : the percent of customers of the state in the commercial sector
- IND.CUST.PCT : the percent of customers of the state in the industrial sector



---

## Cleaning and EDA
### Cleaned DataFrame
The dataframe was loaded from an excel file using pandas' read_excel method. Redudant and irrelevant columns and rows were dropped, for example, the observation number (1, 2, 3 ...).

<iframe
  src="assets/df_head.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

|    |   YEAR |   MONTH | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | OUTAGE.START.DATE   | OUTAGE.START.TIME   | OUTAGE.RESTORATION.DATE   | OUTAGE.RESTORATION.TIME   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND |\n|---:|-------:|--------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:--------------------|:--------------------|:--------------------------|:--------------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|\n|  0 |   2011 |       7 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | 2011-07-01 00:00:00 | 17:00:00            | 2011-07-03 00:00:00       | 20:00:00                  | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 | 2.33292e+06 | 2.11477e+06 | 2.11329e+06 |   6.56252e+06 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |\n|  1 |   2014 |       5 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | 2014-05-11 00:00:00 | 18:38:00            | 2014-05-11 00:00:00       | 18:39:00                  | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 | 1.58699e+06 | 1.80776e+06 | 1.88793e+06 |   5.28423e+06 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |\n|  2 |   2010 |      10 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | 2010-10-26 00:00:00 | 20:00:00            | 2010-10-28 00:00:00       | 22:00:00                  | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 | 1.46729e+06 | 1.80168e+06 | 1.9513e+06  |   5.22212e+06 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |\n|  3 |   2012 |       6 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | 2012-06-19 00:00:00 | 04:30:00            | 2012-06-20 00:00:00       | 23:00:00                  | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 | 1.85152e+06 | 1.94117e+06 | 1.99303e+06 |   5.78706e+06 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |\n|  4 |   2015 |       7 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | 2015-07-18 00:00:00 | 02:00:00            | 2015-07-19 00:00:00       | 07:00:00                  | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 | 2.02888e+06 | 2.16161e+06 | 1.77794e+06 |   5.97034e+06 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |



### Univariate Analysis
<iframe
  src="assets/state_counts.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Here we can see there is a disproportionate number of outages in California (210) and Texas (127). The number of outages by state seems like it coud be related to the size and population of the state.

<iframe
  src="assets/cause_counts.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
We can see severe weather makes up about half of total outages, followed by intentional attacks.

### Bivariate Anaylsis

<iframe
  src="assets/dur_cause_bar.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Here we can see the dataset is heavily skewed. Most causes other than severe weather have a majority of durations towards the relatively lower end of the scale. (Feel free to zoom in and pan around)

<iframe
  src="assets/customer_vs_demand.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Here we can see there is a weak positive correlation between the demand loss of an outage and the number of customers affected.

### Aggregate Analysis
Here we have the proportion of outages in each cause category by region:

<iframe
  src="assets/pivot_region_cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

| CLIMATE.REGION     |   equipment failure |   fuel supply emergency |   intentional attack |    islanding |   public appeal |   severe weather |   system operability disruption |\n|:-------------------|--------------------:|------------------------:|---------------------:|-------------:|----------------:|-----------------:|--------------------------------:|\n| Central            |           0.035     |              0.02       |            0.19      |   0.015      |       0.01      |         0.675    |                       0.055     |\n| East North Central |           0.0217391 |              0.0362319  |            0.144928  |   0.00724638 |       0.0144928 |         0.753623 |                       0.0217391 |\n| Northeast          |           0.0142857 |              0.04       |            0.385714  |   0.00285714 |       0.0114286 |         0.502857 |                       0.0428571 |\n| Northwest          |           0.0151515 |              0.00757576 |            0.674242  |   0.0378788  |       0.0151515 |         0.219697 |                       0.030303  |\n| South              |           0.0436681 |              0.0305677  |            0.122271  |   0.00873362 |       0.183406  |         0.49345  |                       0.117904  |\n| Southeast          |           0.0326797 |            nan          |            0.0588235 | nan          |       0.0326797 |         0.771242 |                       0.104575  |\n| Southwest          |           0.0543478 |              0.0217391  |            0.695652  |   0.0108696  |       0.0108696 |         0.108696 |                       0.0978261 |\n| West               |           0.0967742 |              0.078341   |            0.142857  |   0.129032   |       0.0414747 |         0.322581 |                       0.18894   |\n| West North Central |           0.0588235 |              0.0588235  |            0.235294  |   0.294118   |       0.117647  |         0.235294 |                     nan         |

This chart helps us understand the distribution of outage causes across different regions. Since the distributions look different, region could be a good feature for a predictive model of cause.

---

## Assessment of Missingness

### NMAR Analysis
DEMAND.LOSS.MW could be Not Missing At Random (NMAR) with the reasoning that if demand loss is very low, it is less likely to be reported and present in the dataset. It's missiningness might be explained with number of CUSTOMERS.AFFECTED, based on the correlation in the bivariate analysis, but it would make sense for these to both be NMAR. 

### Missingness Dependency (MAR Analysis)
CUSTOMERS.AFFECTED has 443 missing values (29% of observations). This seems like an important piece of information so we would like to investigate whether it's missingness is dependent on other columns in this dataset. 

We ran two permutation tests on #1 OUTAGE.DURATION and #2 RES.SALES as possible factors the CUSTOMERS.AFFECTED missingness could be dependent on.

#1 OUTAGE.DURATION

<iframe
  src="assets/miss_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Here we can see the distribution of outage duration based on if customers affected is missing or not. They seem different enough that there may be a dependency here. Let's see with the permutation test.

Our test statistic was the difference in the means of OUTAGE.DURATION when CUSTOMERS.AFFECTED was missing and when it was not missing.

<iframe
  src="assets/perm_out.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
We chose a significance level of 0.05. We reject the null hypothesis that CUSTOMERS.AFFECTED is not dependent on OUTAGE.DURATION with a p-value of 0.0062. This leads us to believe it's missingness mechanism is Missing at Random (MAR) dependent on OUTAGE.DURATION.

#2 RES.SALES

Our test statistic was the difference in the means of RES.SALES when CUSTOMERS.AFFECTED was missing and when it was not missing.

<iframe
  src="assets/perm_sale.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
We chose a significance level of 0.05. We fail to reject the null hypothesis that CUSTOMERS.AFFECTED is not dependent on RES.SALES with a p-value of 0.1298.

These results make sense because one can reason that the shorter an outage lasts, the less likely many customers were affected, and so fewer values are reported. It is possible lower residential sales values mean fewer people would be affected by an outage, but the relationship doesn't seem as direct here.

Interestingly, we found customers affected missingness could be dependent on outage duration, but not the other way around. Meaning duration missingness is likely not dependent on number of customers affected, with a p-value of 0.3874.

### Missing Value Imputation
Alaska and Hawaii both have the only missing values for CLIMATE.REGION, so they were filled with 'Non-Contiguous'
TOTAL.PRICE, TOTAL.SALES, and ANOMALY.LEVEL were mean imputed since there were only 9-22 mssing values.


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
To improve on our baseline we added the following features:
- TOTAL.CUSTOMERS standardized by CLIMATE.REGION
- TOTAL.PRICE
- TOTAL.SALES
- ANOMALY.LEVEL
- TOTAL.REALGSP standardized by U.S.\_STATE
- RES.CUST.PCT
- COM.CUST.PCT
- IND.CUST.PCT
- U.S.\_STATE one-hot-encoded
and kept the NERC.REGION and CLIMATE.REGION features

We added these features because they help describe the demographic of place the outage occured, and they include information we would have before knowing if an outage was definitely caused by an intentional attack.

We used gridsearch to find optimal hyperparameters for our model, which were 16 for the maximum depth of the decision trees, and 2 for the minumum sample split.

The accuracy of this model is 87.76%, but more importantly it's F1 score is 0.791 which are both great improvements from the baseline.

---

## Fairness Testing
We tested whether the model performs fairly on outages that occured in places with low vs high population (above and below 8769252 inhabitants, the median population in our dataset). Our evaluation metric was accuracy.

Null Hypothesis: The model has the same accuracy on cases with low populations and high populations.

Alternative Hypothesis: The model has different accuracy on cases with low populations and high populations.

The test statistic we used for a permutation test was the absolute value of low population accuracy - high population accuracy.

We chose a significance level of 0.05 and the p-value is 0.553, so we fail to reject the null hypothesis. We can be more confident that our model performs similarly on these two groups.


---