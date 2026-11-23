# Beyond the Purchase: Cohort Retention & Churn Analysis — Project Report

## Overview

For this project, I wanted to understand what happens after a customer makes a first purchase. A transaction report can show how much was sold, but it does not automatically explain whether customers come back or how customer activity changes over time.

I used Python to analyse a historical retail transaction dataset, build monthly customer cohorts, calculate retention, and explore an inactivity-based churn measure.

## The business problem

A retailer may continue acquiring new customers while losing existing ones. Without a customer-level view, it is difficult to see whether customers return after their first purchase, which acquisition groups remain active, or which behaviours are associated with long periods without a purchase.

I focused on these questions:

- How many customers return after their first purchase?
- How does retention change across monthly cohorts?
- What percentage of customers have been inactive for more than 90 days?
- Does the inactivity rate differ according to purchase frequency?

## What I did

I started by loading the Excel workbook with pandas and checking the transaction data. I kept records marked as Approved and converted the transaction date into a proper datetime field. This left 19,821 approved transaction rows from 3,493 customers.

Next, I assigned every customer to the month of their first approved transaction. I compared each customer's later activity with that first month and calculated a cohort index. This allowed me to build a retention matrix and display the results as a heatmap.

I then created a customer-level summary containing the last transaction date, transaction count, total list-price value and online-order share. Using the final date in the dataset as the reference point, I calculated how many days had passed since each customer's last transaction. I used more than 90 days of inactivity as a working churn proxy.

Finally, I compared inactivity across transaction-count groups and trained a basic logistic-regression model as an exploratory exercise.

## What I found

The analysis reports average month-one retention of 35.6% across the monthly cohorts. The inactivity proxy identifies 23.1% of customers as having gone more than 90 days without an approved transaction by the end of the observed period.

The clearest descriptive pattern appears when customers are grouped by transaction frequency. The inactivity rate is 69.4% for customers with one recorded transaction, 58.8% for customers with two, and 38.0% for customers with three. It continues to decline among higher-frequency groups.

This suggests that repeat purchasing and customer engagement deserve attention. It does not prove that simply increasing the number of purchases will prevent churn; the relationship may also reflect the fact that customers who are already more engaged naturally make more purchases.

## Model result and what it taught me

The logistic-regression baseline achieved a ROC-AUC of 0.724 on a random holdout sample. However, churn recall was only 0.199, which means the model missed most customers labelled as inactive.

I also identified a design issue in the current modelling approach: the customer features are calculated across the same period used to define inactivity. That means the model is not truly predicting a future event from past behaviour. I would not present it as a production-ready churn model.

For a proper predictive version, I would choose a snapshot date, calculate customer features using only earlier transactions, and define churn from a separate future period. I would then use a chronological validation split.

## Recommendations

1. Track first-to-second purchase conversion as a core customer-retention measure.
2. Study the time between a customer's first and second purchases to identify an appropriate follow-up window.
3. Compare retention across acquisition cohorts and product categories.
4. Test re-engagement campaigns on carefully defined inactive segments and measure incremental outcomes.
5. Rebuild the churn model with a time-based design before using predictions for customer outreach.

## What I learned

This project helped me understand how to move from transaction-level data to customer-level analysis. I practised date manipulation, cohort assignment, retention calculations, aggregation, customer segmentation, visualisation and basic classification.

More importantly, I learned that a model score is not enough to establish that a model is useful. The target definition, timing of features, class-level recall and business context matter just as much as the algorithm.

## Limitations

The analysis uses a historical one-year period and an inactivity threshold selected for this exercise. The dataset's original provenance and reuse licence have not been independently confirmed. The transaction-row grain should also be checked before treating every row as a complete customer order. These limitations are documented in [Limitations and Next Steps](LIMITATIONS_AND_NEXT_STEPS.md).
