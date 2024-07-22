---
layout: post
title: Assessing Campaign Performance Using Chi-Square Test For Independence
image: "/posts/ab-testing-title-img.png"
tags: [AB Testing, Hypothesis Testing, Chi-Square, Python]
---

In this project we apply Chi-Square Test For Independence (a Hypothesis Test) to assess the performance of two types of mailers that were sent out to promote a new service! 

# Table of contents

- [00. Project Overview](#overview-main)
    - [Context](#overview-context)
    - [Actions](#overview-actions)
    - [Results & Discussion](#overview-results)
- [01. Concept Overview](#concept-overview)
- [02. Data Overview & Preparation](#data-overview)
- [03. Applying Chi-Square Test For Independence](#chi-square-application)
- [04. Analysing The Results](#chi-square-results)
- [05. Discussion](#discussion)

___

# Project Overview  <a name="overview-main"></a>

### Context <a name="overview-context"></a>

Our client, a grocery retailer, promoted their new "Delivery Club," a $100 annual membership offering free deliveries (normally $10 each).

Customers were randomly assigned to three groups: one received a low-cost mailer, another a high-cost mailer, and the third (control) received no mailer.

The client observed higher sign-up rates among contacted customers compared to the control group and now seeks to determine if there is a significant difference in sign-up rates between the cheap and expensive mailers. This analysis will help optimize future campaign ROI.


<br>
<br>
### Actions <a name="overview-actions"></a>

For this test, focused on comparing the rates of two groups, we used the Chi-Square Test for Independence. Full details of this test are provided below.

**Note:** We could also use the Z-Test for Proportions, but chose the Chi-Square Test because:

* Both tests yield the same test statistic
* The Chi-Square Test can be represented using 2x2 tables, making it easier to explain to stakeholders
* The Chi-Square Test can extend to more than two groups, allowing a consistent approach for the client

From the *campaign_data* table in the client database, we isolated customers who received "Mailer 1" (low cost) and "Mailer 2" (high cost), excluding the control group.

Our hypotheses and acceptance criteria are as follows:

**Null Hypothesis:** There is no relationship between mailer type and signup rate. They are independent.
**Alternate Hypothesis:** There is a relationship between mailer type and signup rate. They are not independent.
**Acceptance Criteria:** 0.05

We aggregated the data into a 2x2 matrix for *signup_flag* by *mailer_type* and used the scipy library to calculate the Chi-Square statistic, p-value, degrees of freedom, and expected values.
<br>
<br>

### Results & Discussion <a name="overview-results"></a>

Based on our observed values, the signup rates for each group are:

* Mailer 1 (Low Cost): **32.8%** signup rate
* Mailer 2 (High Cost): **37.8%** signup rate

However, the Chi-Square Test results are:

* Chi-Square Statistic: **1.94**
* p-value: **0.16**

The Critical Value for our specified Acceptance Criteria of 0.05 is **3.84**

Since the Chi-Square statistic is less than the critical value, we retain the null hypothesis and conclude there is no significant relationship between mailer type and signup rate.

Although Mailer 2 showed a higher signup rate (37.8%) compared to Mailer 1 (32.8%), this difference is not significant at the 0.05 level. Without this hypothesis test, the client might have mistakenly assumed that higher-cost mailers are always better. This test suggests that spending more on mailers *may not necessarily* result in increased revenue.

Our results *do not definitively state there is no difference between the two mailers*, but rather that more data and further A/B testing are needed to gain more insights and draw firm conclusions.

<br>
<br>

___

# Concept Overview  <a name="concept-overview"></a>

<br>
#### A/B Testing

An A/B Test is a randomized experiment with two groups, A and B, receiving different experiences. We measure each group's response to inform future business decisions.

Applications include testing online ad strategies, email subject lines, or the effect of mailing coupons versus a control group. Companies like Amazon and Netflix use these tests continuously to optimize their offerings.

<br>
#### Hypothesis Testing

A Hypothesis Test assesses the likelihood of an assumed viewpoint based on sample data, helping determine if a belief about the data is true.

<br>
**The Null Hypothesis**

We start with the Null Hypothesis, which states there is no relationship or effect, attributing any observed result to chance.

<br>
**The Alternate Hypothesis**

The test seeks evidence to support or reject the Null Hypothesis. Rejecting it supports the Alternate Hypothesis, indicating a relationship or effect exists.

<br>
**The Acceptance Criteria**

Before collecting data, we set an Acceptance Criteria, a p-value threshold to decide whether to reject the Null Hypothesis. Typically set at 0.05, this value can be adjusted based on the required confidence level.

In summary, a Hypothesis Test evaluates the Null Hypothesis using a p-value against the Acceptance Criteria.

<br>
**Types Of Hypothesis Test**

Different Hypothesis Tests suit various scenarios based on data type and the questions asked. In the case of our task here, where we are looking to understand the difference in sign-up *rate* between two groups - we utilise the Chi-Square Test For Independence.

<br>
#### Chi-Square Test For Independence

The Chi-Square Test for Independence checks if observed frequencies for categorical variables match expected frequencies under the Null Hypothesis, assuming no difference between groups.

Observed frequencies are the actual values seen, while expected frequencies are what we would expect based on all data.

**Note:** Another option when comparing "rates" is a test known as the *Z-Test For Proportions*.  While, we could absolutely use this test here, we have chosen the Chi-Square Test For Independence because:

* The resulting test statistic for both tests will be the same
* The Chi-Square Test can be represented using 2x2 tables of data - meaning it can be easier to explain to stakeholders
* The Chi-Square Test can extend out to more than 2 groups - meaning the business can have one consistent approach to measuring signficance

___

<br>
# Data Overview & Preparation  <a name="data-overview"></a>

In the client database, the campaign_data table shows which customers received each type of "Delivery Club" mailer, which customers were in the control group, and which customers joined the club.

We aim to find evidence that the signup rate for "Mailer 1" (low cost) differs from "Mailer 2" (high cost). Thus, we'll extract customers from these two groups and exclude the control group.

The code below:

* Loads the necessary Python libraries for data import and the chi-square test (using scipy)
* Imports the data from the *campaign_data* table
* Excludes control group customers, leaving only Mailer 1 and Mailer 2 customers

<br>
```python

# install the required python libraries
import pandas as pd
from scipy.stats import chi2_contingency, chi2

# import campaign data
campaign_data = ...

# remove customers who were in the control group
campaign_data = campaign_data.loc[campaign_data["mailer_type"] != "Control"]

```
<br>
A sample of this data (the first 10 rows) can be seen below:
<br>
<br>

| **customer_id** | **campaign_name** | **mailer_type** | **signup_flag** |
|---|---|---|---|
| 74 | delivery_club | Mailer1 | 1 |
| 524 | delivery_club | Mailer1 | 1 |
| 607 | delivery_club | Mailer2 | 1 |
| 343 | delivery_club | Mailer1 | 0 |
| 322 | delivery_club | Mailer2 | 1 |
| 115 | delivery_club | Mailer2 | 0 |
| 1 | delivery_club | Mailer2 | 1 |
| 120 | delivery_club | Mailer1 | 1 |
| 52 | delivery_club | Mailer1 | 1 |
| 405 | delivery_club | Mailer1 | 0 |
| 435 | delivery_club | Mailer2 | 0 |

<br>
In the DataFrame we have:

* customer_id
* campaign name
* mailer_type (either Mailer1 or Mailer2)
* signup_flag (either 1 or 0)

___

<br>
# Applying Chi-Square Test For Independence <a name="chi-square-application"></a>

<br>
#### State Hypotheses & Acceptance Criteria For Test

The first step in any Hypothesis Test is to state the Null Hypothesis, Alternate Hypothesis, and Acceptance Criteria (detailed above).

In the code below, we explicitly define these to explain the results later, using the common Acceptance Criteria value of 0.05.

```python

# specify hypotheses & acceptance criteria for test
null_hypothesis = "There is no relationship between mailer type and signup rate.  They are independent"
alternate_hypothesis = "There is a relationship between mailer type and signup rate.  They are not independent"
acceptance_criteria = 0.05

```

<br>
#### Calculate Observed Frequencies & Expected Frequencies

As mentioned above, in a Chi-Square Test for Independence, the observed frequencies are the actual rates per group, while the expected frequencies are based on all combined data.

The code below:

* Summarises our dataset to a 2x2 matrix for *signup_flag* by *mailer_type*
* Based on this, calculates the:
    * Chi-Square Statistic
    * p-value
    * Degrees of Freedom
    * Expected Values
* Prints out the Chi-Square Statistic & p-value from the test
* Calculates the Critical Value based upon our Acceptance Criteria & the Degrees Of Freedom
* Prints out the Critical Value

```python

# aggregate our data to get observed values
observed_values = pd.crosstab(campaign_data["mailer_type"], campaign_data["signup_flag"]).values

# run the chi-square test
chi2_statistic, p_value, dof, expected_values = chi2_contingency(observed_values, correction = False)

# print chi-square statistic
print(chi2_statistic)
>> 1.94

# print p-value
print(p_value)
>> 0.16

# find the critical value for our test
critical_value = chi2.ppf(1 - acceptance_criteria, dof)

# print critical value
print(critical_value)
>> 3.84

```
<br>
Based on our observed values, the signup rates are:

* Mailer 1 (Low Cost): **32.8%** 
* Mailer 2 (High Cost): **37.8%**

While the higher cost mailer shows a higher signup rate, the Chi-Square Test results will indicate if this difference is statistically significant or due to chance.

Our Chi-Square Statistic is **1.94** with a p-value of **0.16**. The critical value for an Acceptance Criteria of 0.05 is **3.84**.

Note: We applied the Chi-Square Test with *correction = False*, known as *Yate's Correction*, to prevent overestimation of statistical significance when Degrees of Freedom is one.

___

<br>
# Analyzing The Results <a name="chi-square-results"></a>

We have everything needed to interpret our Chi-Square test results. With a p-value of **0.16** *greater* than the Acceptance Criteria of 0.05, we *retain* the Null Hypothesis and conclude there is no significant difference between the signup rates of Mailer 1 and Mailer 2.

Similarly, the Chi-Square statistic of **1.94** is *lower* than the Critical Value of **3.84**, leading to the same conclusion.

To make this script more dynamic, we can create code to automatically interpret and explain the results.

```python

# print the results (based upon p-value)
if p_value <= acceptance_criteria:
    print(f"As our p-value of {p_value} is lower than our acceptance_criteria of {acceptance_criteria} - we reject the null hypothesis, and conclude that: {alternate_hypothesis}")
else:
    print(f"As our p-value of {p_value} is higher than our acceptance_criteria of {acceptance_criteria} - we retain the null hypothesis, and conclude that: {null_hypothesis}")

>> As our p-value of 0.16351 is higher than our acceptance_criteria of 0.05 - we retain the null hypothesis, and conclude that: There is no relationship between mailer type and signup rate.  They are independent


# print the results (based upon p-value)
if chi2_statistic >= critical_value:
    print(f"As our chi-square statistic of {chi2_statistic} is higher than our critical value of {critical_value} - we reject the null hypothesis, and conclude that: {alternate_hypothesis}")
else:
    print(f"As our chi-square statistic of {chi2_statistic} is lower than our critical value of {critical_value} - we retain the null hypothesis, and conclude that: {null_hypothesis}")
    
>> As our chi-square statistic of 1.9414 is lower than our critical value of 3.841458820694124 - we retain the null hypothesis, and conclude that: There is no relationship between mailer type and signup rate.  They are independent

```
<br>
The output confirms that we retain the Null Hypothesis, finding no significant difference in signup rates between Mailer 1 and Mailer 2.

___

<br>
# Discussion <a name="discussion"></a>

While Mailer 2 had a higher signup rate (37.8%) than Mailer 1 (32.8%), this difference is not significant at an Acceptance Criteria of 0.05.

Without this Hypothesis Test, the client might have assumed higher cost mailers are always better, which could lead to unnecessary expenses without guaranteed revenue increase.

Our results *do not definitively state there is no difference between the mailers*, only that we should not draw rigid conclusions yet.

More A/B tests and data collection will provide further insights.
