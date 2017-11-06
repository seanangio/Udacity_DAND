# P7
19 May 2017  



# Experiment Design
## Metric Choice

*For each metric, explain both why you did or did not use it as an invariant metric and why you did or did not use it as an evaluation metric. Also, state what results you will look for in your evaluation metrics in order to launch the experiment.*

**Selections:**
* Invariant Metrics: number of cookies, number of clicks, click-through-probability
* Evaluation Metrics: gross conversion, retention, net conversion

**Explanations:**
Number of cookies (number of unique cookies to view course overview page)
* An invariant metric because it is a metric that is evenly distributed between the control and experiment groups, and it maintains independence given that visits occur before the experiment. It can provide a sanity check.
* Not an evaluation metric because, compared to other possible metrics, it is not particularly useful in answering the question of the experiment. By itself, it does not provide any insight into the high level business concept we are investigating. 

Number of user-ids (number of users who enroll in the free trial)
* Not an invariant metric because the number of users who enroll in the free trial is dependent on the experiment, and so we would have different numbers of users in the control and experiment groups. In other words, it lacks the independence needed for an invariant metric. 
* Not an evaluation metric because, compared to other possible metrics, it is not particularly useful in answering the question of the experiment. By itself, it does not provide any insight into the high level business concept we are investigating. 

Number of clicks (number of unique cookies to click the “Start free trial” button)
* An invariant metric because clicks on the “Start free trial” button happen before the user sees the experiment, and is thus an independent metric.
* Not an evaluation metric because, compared to other possible metrics, it is not particularly useful in answering the question of the experiment. By itself, it does not provide any insight into the high level business concept we are investigating. 

Click-through Probability (number of unique cookies to click the “Start free trial” button divided by number of unique cookies to view the course overview page)
* Yes an invariant metric because it is calculated by simply dividing two other invariant metrics and therefore is similarly independent. 
* Not an evaluation metric because, compared to other possible metrics, it is not particularly useful in answering the question of the experiment. By itself, it does not provide any insight into the high level business concept we are investigating. 

Gross Conversion (number of user ids to complete checkout and enroll in the free trial divided by the number of unique cookies to click the “Start free trial” button)
* Not an invariant metric because the numerator of the metric is dependent on the experiment.
* Yes an evaluation metric because the value of this metric is directly dependent on the effect of the experiment. By comparing results across the experiment and control groups, we will be able to demonstrate the extent to which the cost of enrollments that fail to become paying customers decreases. This metric gets to the heart of the question the experiment is trying to answer, and so is a good evaluation metric. 

Retention (number of user ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of user ids to complete checkout)
* Not an invariant metric because the number of users who enroll in the free trial is dependent on the experiment.
* Yes an evaluation metric because it helps us evaluate the central question of the experiment. In the experiment group, we hypothesize that greater awareness of the time commitment will lead to higher retention than in the control group. The metric is directly dependent on effect of experiment and gives insight to the business case we hope to investigate. 

Net conversion (number of user ids to remain enrolled past the 14 day boundary (and thus make at least one payment) divided by the number of unique cookies to click the “Start free trial” button) 
* Not an invariant metric because the number of users who enroll in the free trial is dependent on the experiment.
* Yes an evaluation metric because, as with the other evaluation metrics, it helps evaluate the central hypothesis of the experiment. It is directly dependent on the effect of the experiment and provides business case insight. 

*Also, state what results you will look for in your evaluation metrics in order to launch the experiment.*

Before launching the experiment, I would expect gross conversion to decrease with practical significance. This would indicate lower costs due to the screener helping to filter out less serious candidates. Net conversion, on the other hand, we would hope to not decrease with statistical significance. In other words, we want revenues to be unaffected by the screener described in the experiment. We might also expect retention to increase with practical significance.

## Measuring Standard Deviation
*List the standard deviation of each of your evaluation metrics. (These should be the answers from the "Calculating standard deviation" quiz.)
Quiz: Calculating standard deviation*


```r
unique_cookies_to_view <- 40000
unique_cookies_to_click <- 3200
enrollments_per_day <- 660
sample_size <- 5000

N <- sample_size * (unique_cookies_to_click / unique_cookies_to_view)

p_enrolling_given_click <- enrollments_per_day / unique_cookies_to_click
p_payment_given_enroll <- 0.53
p_payment_given_click <- p_enrolling_given_click * p_payment_given_enroll

N_retention <- sample_size * (unique_cookies_to_click/unique_cookies_to_view) * p_enrolling_given_click

se_gross_conversion <- sqrt((p_enrolling_given_click * (1 - p_enrolling_given_click))/N)
se_gross_conversion
```

```
## [1] 0.0202306
```

```r
se_retention <- sqrt((p_payment_given_enroll * (1 - p_payment_given_enroll))/N_retention)
se_retention
```

```
## [1] 0.05494901
```

```r
se_net_conversion <- sqrt((p_payment_given_click * (1 - p_payment_given_click))/N)
se_net_conversion
```

```
## [1] 0.01560154
```


**Output:**
Gross conversion: 0.0202306
Retention: 0.05494901
Net Conversion: 0.01560154

*For each of your evaluation metrics, indicate whether you think the analytic estimate would be comparable to the the empirical variability, or whether you expect them to be different (in which case it might be worth doing an empirical estimate if there is time). Briefly give your reasoning in each case.*

I expect the analytic estimate to be comparable to the empirical variability because the unit of analysis (in this case, cookies) is the same as the unit of diversion in the case of gross and net conversion.

In the case of the metric of retention, the unit of analysis and unit of diversion are not the same. Thus suggests that an empirical estimate of variability is preferable. However, for reasons described in the section below, we see that this metric is not feasible for the study and so will be dropped from consideration. 

## Sizing
**Number of Samples vs. Power**
*Indicate whether you will use the Bonferroni correction during your analysis phase, and give the number of pageviews you will need to power you experiment appropriately. (These should be the answers from the "Calculating Number of Pageviews" quiz.)*


Will you use the Bonferroni correction in your analysis phase?
* No
Which evaluation metrics did you select?
* Gross conversion, Net conversion
How many page views will you need?
* 685325


I did not use the Bonferroni correction during the analysis phase because the Bonferroni correction can be too conservative when test metrics are positively correlated, and in this case, they are.


Using Evan Miller’s sample size calculator with alpha set to 0.05 and 1 - beta to 0.2 yields the following results:
* Gross Conversion:
   * Baseline conversion rate = 20.625%
   * D_min = 1%
      * Sample size: 25835
         * Which we divide by 0.08 (click-through-probability on “Start free trial”) and multiply by 2 (control + experiment groups) to get 645875
* Retention:
   * Baseline conversion rate = 53%
   * D_min = 1%
      * Sample size: 39115
         * Which we divide by 0.08 (click-through-probability on “Start free trial”), divide by 0.20625 (probability of enrolling given click),  and multiply by 2 (control + experiment groups) to get 4741212
* Net conversion:
   * Baseline conversion rate = 10.93125%
   * D_min = 0.75%
      * Sample size: 27413
         * Which we divide by 0.08  (click-through-probability on “Start free trial”) and multiply by 2 (control + experiment groups) to get 685325


The necessary sample size for retention is too large to be feasible for the scope of the experiment and can be dropped from consideration in the context of this experiment. It would take too long of a duration to acquire a large enough sample.


Of the remaining evaluation metrics, I will take the larger value of 685,325 for net conversion.


**Duration vs. Exposure**
Indicate what fraction of traffic you would divert to this experiment and, given this, how many days you would need to run the experiment. (These should be the answers from the "Choosing Duration and Exposure" quiz.)


Number of pageviews: 685325
Fraction of traffic exposed: 0.6
Length of experiment: 29


Experiment_length = page_views / (unique_cookies_to_view_page_per_day * fraction_exposed)


Length of experiment as adjusted by fraction diverted:
* 685325 / (40000 * 0.3) = 57.11 days
* 685325 / (40000 * 0.4) = 42.83 days
* 685325 / (40000 * 0.5) = 34.27 days
* **685325 / (40000 * 0.6) = 28.56 days**
* 685325 / (40000 * 0.7) = 24.48 days


*Give your reasoning for the fraction you chose to divert. How risky do you think this experiment would be for Udacity?*


If we choose to divert 60% of our traffic, the experiment will take 29 days. 30% of traffic will be placed in a control group and 30% in the experimental group. Exposing 30% of traffic to the change described in the experiment seems like a reasonable risk given our goals because although the experiment does not affect paying customers, it could impact new enrollments. Selecting this fraction also has the advantage of keeping our experiment to 29 days, which is just over four weeks, and perhaps help account for natural variation over the course of a given month.


# Experiment Analysis
## Sanity Checks

*For each of your invariant metrics, give the 95% confidence interval for the value you expect to observe, the actual observed value, and whether the metric passes your sanity check. (These should be the answers from the "Sanity Checks" quiz.)*

*For any sanity check that did not pass, explain your best guess as to what went wrong based on the day-by-day data. Do not proceed to the rest of the analysis unless all sanity checks pass.*


Invariant metrics: number of cookies, number of clicks, click-through-probability


Number of cookies
* Total Control group pageview: 345543 (from spreadsheet)
* Total Experiment group pageview: 344660 (from spreadsheet)
* Total pageview: 345543 + 344660 = 690203
* Standard Deviation = sqrt(0.5 * 0.5 / (345543 + 344660)) = 0.0006018
* Margin of Error = 1.96 * 0.0006018 = 0.0011796
* Lower Bound = 0.5 - 0.0011797 = 0.4988
* Upper Bound = 0.5 + 0.0011797 = 0.5012
* Observed = 345543 / (345543 + 344660) = 0.5006 (PASS)


Number of clicks
* Total Control group clicks = 28378 (from spreadsheet)
* Total Experiment group clicks = 28325 (from spreadsheet)
* Standard Deviation = sqrt(0.5 * 0.5 / (28378 + 28325)) = 0.0021
* Margin of Error = 1.96 * 0.0021 = 0.0041
* Lower Bound = 0.5 - 0.0041 = 0.4959
* Upper Bound = 0.5 + 0.0041 = 0.5041
* Observed = 28378 / (28378 + 28325) = 0.5005 (PASS)


Click-through Probability
* Control value = control group clicks / (control group pageviews
   * 28378 / 345543 = 0.08212581357
* Standard Deviation = sqrt(0.08212581357  * (1-0.08212581357) / 344660) = 0.0004676661955
* Margin of Error = 1.96 * 0.0004676661955 = 0.0009166257431
* Lower Bound = 0.08212581357 - 0.0009166257431 = 0.08120918783
* Upper Bound = 0.08212581357 + 0.0009166257431= 0.08304243932
* Experiment Value = (28325 / 344660) = 0.08218244067 (PASS)


## Result Analysis

**Effect Size Tests**

*For each of your evaluation metrics, give a 95% confidence interval around the difference between the experiment and control groups. Indicate whether each metric is statistically and practically significant. (These should be the answers from the "Effect Size Tests" quiz.)*

Did you use the Bonferroni correction? No

Gross conversion: 
* Lower Bound: -0.02912336
* Upper Bound: -0.01198639
* Statistical Significance: Yes
* Practical Significance: Yes

Net conversion: 
* Lower Bound: -0.01160462
* Upper Bound: 0.001857179
* Statistical Significance: No
* Practical Significance: No



```r
# from spreadsheet
N_cnt <- 17293
N_exp <- 17260
enrollments_cnt <- 3785
enrollments_exp <- 3423
payments_cnt <- 2033
payments_exp <- 1945


p_pooled_gc <- (enrollments_cnt + enrollments_exp) / (N_cnt + N_exp)
p_pooled_nc <- (payments_cnt + payments_exp) / (N_cnt + N_exp)


se_pooled_gc <- sqrt((p_pooled_gc * (1 - p_pooled_gc))*((1/N_cnt) + (1/N_exp)))
se_pooled_nc <- sqrt((p_pooled_nc * (1 - p_pooled_nc))*((1/N_cnt) + (1/N_exp)))

d_gc <- (enrollments_exp / N_exp) - (enrollments_cnt / N_cnt)
d_nc <- (payments_exp / N_exp) - (payments_cnt / N_cnt)


m_gc <- se_pooled_gc * 1.96
m_nc <- se_pooled_nc * 1.96

lower_gc <- d_gc - m_gc
lower_nc <- d_nc - m_nc

upper_gc <- d_gc + m_gc
upper_nc <- d_nc + m_nc

print("Gross Conversion")
```

```
## [1] "Gross Conversion"
```

```r
cat("Lower Bound:", lower_gc)
```

```
## Lower Bound: -0.02912336
```

```r
cat("Upper Bound:", upper_gc)
```

```
## Upper Bound: -0.01198639
```

```r
print("Net Conversion")
```

```
## [1] "Net Conversion"
```

```r
cat("Lower Bound:", lower_nc)
```

```
## Lower Bound: -0.01160462
```

```r
cat("Upper Bound:", upper_nc)
```

```
## Upper Bound: 0.001857179
```


**Sign Tests**
*For each of your evaluation metrics, do a sign test using the day-by-day data, and report the p-value of the sign test and whether the result is statistically significant. (These should be the answers from the "Sign Tests" quiz.)*


Did you use the Bonferroni correction? No


We get the following results from an online calculator:


Gross Conversion:
Number of "successes": 4 
Number of trials (or subjects) per experiment: 23 
Sign test. If the probability of "success" in each trial or subject is 0.500, then:
* The one-tail P value is 0.0013 
* This is the chance of observing 4 or fewer successes in 23 trials.
* The two-tail P value is 0.0026 
* This is the chance of observing either 4 or fewer successes, or 19 or more successes, in 23 trials.
This result is statistically significant because p-value = 0.0026 < alpha = 0.05


Net Conversion:
Number of "successes": 10 
Number of trials (or subjects) per experiment: 23 
Sign test. If the probability of "success" in each trial or subject is 0.500, then:
* The one-tail P value is 0.3388 
* This is the chance of observing 10 or fewer successes in 23 trials.
* The two-tail P value is 0.6776 
* This is the chance of observing either 10 or fewer successes, or 13 or more successes, in 23 trials.
This result is NOT statistically significant because p-value = 0.6776 > alpha = 0.05


**Summary**
*State whether you used the Bonferroni correction, and explain why or why not. If there are any discrepancies between the effect size hypothesis tests and the sign tests, describe the discrepancy and why you think it arose.*


As discussed earlier, I did not use the Bonferroni correction given the choice of our metrics. No discrepancies arose between the effect size hypothesis tests and the sign tests.


## Recommendation

*Make a recommendation and briefly describe your reasoning.*

Based on our results, I would recommend NOT launching the experiment. We received a good result from our first evaluation metric, gross conversion. A negative and practically significant value for gross conversion means that the experiment was effective in lowering the costs of leads that are unlikely to convert into paying customers. However, our results for net conversion were statistically and practically insignificant. The confidence interval also includes negative numbers, which means that the experiment has the potential to decrease revenue. Because it does not appear that this experiment would help the business, I would argue that the risks of implementing the results of the experiment would be too high. 


# Follow-Up Experiment

*Give a high-level description of the follow up experiment you would run, what your hypothesis would be, what metrics you would want to measure, what your unit of diversion would be, and your reasoning for these choices.*

I think anyone searching for online education in the tech industry will find their way to Udacity. Accordingly, I don’t think much effort is needed to increase the width of the initial customer funnel. Like this experiment was designed to do, the challenge is to convert more people taking advantage of free resources into paying for nanodegrees.

There are so many options for online learning that it can be hard to choose from a learner’s point. Sometimes, in my own learning, once I pay for something I become more committed. I have already invested money, and so the investment of my time accordingly follows. 

Instead of enrolling at the end of a free trial, Udacity could offer a small discount to those who pay immediately. By paying upfront, you might have fewer people opting out at the end of the free trial. 

The null hypothesis is that offering an upfront discount will not practically significantly increase the number of paid users compared to existing pricing models. The alternative hypothesis is that the relative number of paid enrollees does increase. 

The evaluation metric for analysis would be the number of new paid enrollees over number of new users. The unit of diversion and unit of analysis would be user ids. This would be the clearest way to identify any change in results. It would seem if someone is a serious user they would create an account so I hope to be able to use user id instead of a cookie.

One potential difficulty with this proposed experiment that would have to be further discussed is the sizing of the experiment. Typically we would roll out such as experiment only to a fraction of traffic. However, with a change in pricing, it would be unfair or discriminatory to offer it only to a portion of users. To minimize risk, you could perhaps first explore the change with surveys of past or potential users. You could also schedule the experiment for a specific narrow time period (essentially a sale window) to limit risk.
