# Customer Cohort Retention & Churn Analysis

A customer-lifecycle analysis of a retail transaction dataset using Python. I used cohort analysis to understand repeat purchasing over time, then examined how purchase frequency relates to inactivity-based churn.

> **Project scope:** This is an analytical portfolio case study using the transaction data in `Online Retail.xlsx`. The workbook contains bicycle-retail product fields (brands, product lines, classes and sizes); the filename alone should not be treated as evidence of the dataset's original publisher or provenance.

## Business problem

Acquiring a customer is only the beginning. A retailer also needs to understand whether customers return, when repeat purchasing drops, and which customer behaviours are associated with inactivity.

The business questions behind this project are:

1. How many customers return after their first purchase?
2. How does retention change month by month for each acquisition cohort?
3. What share of customers had no approved purchase for more than 90 days by the end of the observed period?
4. How does purchase frequency relate to that inactivity measure?
5. Can the available customer-level features distinguish inactive customers, and what are the model's limitations?

## Dataset at a glance

| Item | Value |
|---|---:|
| Observed transaction period | 1 Jan–30 Dec 2017 |
| Rows in the original transaction table | 20,000 |
| Approved transaction rows used | 19,821 |
| Unique customers in the approved data | 3,493 |
| Product brands represented | 6 |
| Product lines represented | 4 |

The analysis filters to records with `order_status = Approved`. The notebook reads the first worksheet in the workbook and uses transaction-level records. See [data dictionary](docs/DATA_DICTIONARY.md) for the fields used.

## Key findings

- **Month-one retention:** The notebook reports an average of **35.6%** across monthly acquisition cohorts, with cohort results ranging from **30.0% to 40.4%**. This is a cohort-level view, not a single overall customer-retention rate.
- **Inactivity proxy:** **23.1%** of the 3,493 customers had a last approved purchase more than 90 days before the dataset's final transaction date.
- **Repeat purchasing is associated with lower inactivity:** The observed inactivity rate falls from **69.4%** among one-transaction customers to **58.8%** among two-transaction customers and **38.0%** among three-transaction customers. The notebook also shows a declining pattern at higher order counts.
- **The baseline classifier is not ready for operational use:** The logistic-regression test ROC-AUC is **0.724**, but churn recall is only **0.199**. It misses most customers labelled as churned, so the score should be treated as an exploratory baseline rather than a deployable prediction system.

These are descriptive findings from the current dataset and definitions. They do not establish that increasing order frequency causes lower churn.

## Cohort retention

I assign each customer to the month of their first approved transaction. Each later transaction is assigned a cohort index representing the number of calendar months since that first-purchase month.

Retention is calculated as:

`Retention at month N = distinct cohort customers active in month N / customers in the original cohort`

![Monthly customer cohort retention heatmap](cohort_retention_heatmap.png)

The heatmap makes it easier to compare acquisition cohorts and see how repeat activity changes as each cohort ages. Later cohorts naturally have fewer observable months, so blank future periods should not be interpreted as zero retention.

## How I approached the analysis

1. **Loaded and inspected the workbook** using pandas.
2. **Filtered approved transactions** and converted transaction dates to datetime.
3. **Built customer cohorts** from each customer's first purchase month.
4. **Calculated cohort retention** using distinct active customers at each month index.
5. **Created customer-level behaviour measures**, including last purchase date, transaction count, total list-price value, online-order share and days since last purchase.
6. **Defined an inactivity label** as more than 90 days since the customer's last approved transaction, measured against the dataset's final transaction date.
7. **Explored purchase-frequency patterns** by comparing the inactivity rate across transaction-count groups.
8. **Trained a logistic-regression baseline** and evaluated it on a stratified 20% holdout sample.

For exact definitions and caveats, see [methodology](docs/METHODOLOGY.md) and [limitations](docs/LIMITATIONS_AND_NEXT_STEPS.md).

## Model evaluation: an important caveat

The notebook reports a test ROC-AUC of **0.724**. However, the current feature set includes total orders, spend, average order value and margin calculated over the same observation window used to determine the inactivity label. This creates target leakage / temporal leakage: information from the full period is used to explain a label derived from that same period.

The low churn recall (**0.199**) is another warning. The model is included to demonstrate a basic classification workflow, not to claim a reliable churn prediction solution. A proper predictive version needs a fixed observation date, features calculated only before that date, and a separate future outcome window.

## Repository contents

| File | Purpose |
|---|---|
| `Online Retail.xlsx` | Transaction-level workbook used by the notebook |
| `cohort_retention.ipynb` | Data preparation, cohort analysis, inactivity analysis and baseline model |
| `cohort_retention_heatmap.png` | Exported cohort-retention visual |
| `requirements.txt` | Python dependencies |
| `docs/BUSINESS_PROBLEM.md` | Business context and analysis questions |
| `docs/DATA_DICTIONARY.md` | Fields and derived measures used |
| `docs/METHODOLOGY.md` | Analysis workflow and metric definitions |
| `docs/KEY_FINDINGS.md` | Findings and interpretation |
| `docs/PROJECT_REPORT.md` | First-person project summary |
| `docs/LIMITATIONS_AND_NEXT_STEPS.md` | Risks, caveats and improvements |
| `docs/REPRODUCING_ANALYSIS.md` | Steps to run the notebook |

## Tools and skills demonstrated

**Python:** pandas, NumPy, Matplotlib, Seaborn, scikit-learn  
**Analysis:** data inspection, date handling, customer segmentation, cohort analysis, retention measurement, feature engineering, logistic regression, model evaluation  
**Business thinking:** customer lifecycle analysis, repeat-purchase behaviour, churn-definition design, interpretation of model limitations

## Run the project

See [Reproducing the analysis](docs/REPRODUCING_ANALYSIS.md) for setup instructions.

## What I would improve next

- Rebuild the churn model using a time-based observation and outcome window to prevent leakage.
- Evaluate class imbalance with precision, recall, F1-score, PR-AUC and a confusion matrix, not ROC-AUC alone.
- Compare inactivity thresholds (for example, 60, 90 and 120 days) and explain how the definition changes the result.
- Add revenue and margin retention by cohort, not just customer-count retention.
- Validate data quality, missing customer identifiers, duplicate transactions and transaction-level business rules before using the results for decisions.

---

*This is an independent portfolio analysis. The reported churn measure is an inactivity proxy based on the final date in this historical dataset; it is not a verified business-defined churn label.*
