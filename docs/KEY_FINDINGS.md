# Key Findings

The findings below are taken from the outputs saved in the current notebook. They should be interpreted within the dataset and definitions documented in this repository.

## 1. Dataset coverage

- 20,000 transaction rows are present in the workbook.
- 19,821 rows have an Approved status and are used in the analysis.
- The approved data contains 3,493 unique customers.
- Transaction dates range from 1 January to 30 December 2017.

## 2. Cohort retention

The README's existing analysis reports average month-one retention of **35.6%** across monthly acquisition cohorts, with cohort values ranging from **30.0% to 40.4%**.

This means that, depending on the acquisition month, roughly three to four out of every ten customers in a cohort made another recorded purchase in the following calendar month. The exact interpretation depends on the transaction-row grain and approved-status filter.

## 3. Inactivity proxy

Using more than 90 days since the last approved transaction as the inactivity rule gives a reported rate of **23.1%**.

The calculation uses the dataset's maximum transaction date as its reference date. It should be described as historical inactivity under a chosen threshold, not as a verified company churn metric.

## 4. Inactivity by transaction frequency

| Transaction rows per customer | Inactivity rate |
|---:|---:|
| 1 | 69.4% |
| 2 | 58.8% |
| 3 | 38.0% |
| 4 | 28.6% |
| 5 | 25.7% |
| 6 | 18.3% |
| 7 | 12.4% |
| 8 | 9.4% |
| 9 | 6.0% |
| 10 or more | 2.4% |

The pattern is strong: customers with more recorded transactions are less likely to meet the 90-day inactivity rule. This is an association, not proof that additional purchases cause retention. Customers who purchased more often have, by definition, demonstrated more activity during the same period.

## 5. Logistic-regression baseline

The saved notebook output reports:

- ROC-AUC: **0.724**
- Churn precision: **0.653**
- Churn recall: **0.199**
- Churn F1-score: **0.305**
- Overall accuracy: **0.791**

The low recall is particularly important: the model identifies only a small share of customers labelled inactive. In addition, the features use full-period customer aggregates and therefore leak information from the same window used to define the target. This model should not be used to trigger retention campaigns or make customer-level decisions.

## Business interpretation

The most useful result is not simply the churn percentage. It is the combination of cohort retention and purchase-frequency patterns: repeat engagement appears to be a key area for further investigation.

A sensible next step would be to examine first-to-second purchase conversion, time to second purchase, and cohort revenue retention. These would help turn the descriptive pattern into more actionable customer-lifecycle analysis.
