# Methodology

## 1. Load and inspect

The notebook uses pandas to read the first worksheet in `Online Retail.xlsx`. It filters the dataset to rows where `order_status` equals `Approved` and converts `transaction_date` to a datetime field.

The executed notebook records 19,821 approved rows and 3,493 unique customers.

## 2. Cohort assignment

Each transaction is assigned a calendar month:

`OrderMonth = transaction_date converted to month`

Each customer is assigned to the month of their earliest approved transaction:

`CohortMonth = minimum OrderMonth for customer_id`

The cohort index measures the number of calendar months since that first purchase month:

`CohortIndex = (transaction year - cohort year) * 12 + (transaction month - cohort month)`

A cohort index of 0 represents the acquisition month, 1 represents the next calendar month, and so on.

## 3. Retention calculation

For each cohort month and cohort index, the notebook counts distinct active customers. It pivots those counts into a cohort matrix and divides each row by that cohort's month-zero customer count.

`Retention(N) = distinct customers from cohort active at month N / original cohort size`

The denominator is the initial cohort size, not the number of customers active in the previous month. This is **period retention**, not a month-over-month survival rate.

Later cohorts have fewer follow-up months in the dataset. Their unobserved future periods must remain blank rather than being treated as zero.

## 4. Customer-level behaviour

The notebook aggregates approved transaction rows by customer to calculate:

- Last transaction date
- Number of transaction rows
- Sum of list price
- Average online-order indicator

It then calculates recency as the number of days between the dataset's latest transaction date and each customer's last transaction date.

## 5. Inactivity / churn proxy

The label is defined as:

`Churned = 1 if recency_days > 90, else 0`

The reference date is the maximum transaction date in the dataset (30 December 2017 in the executed output). The resulting 23.1% is therefore the share of observed customers whose last approved transaction was more than 90 days before that reference date.

This is an **inactivity proxy**. It is not necessarily equivalent to the retailer's official churn definition, and customers near the end of the dataset have less observable follow-up than customers who purchased earlier.

## 6. Frequency analysis

The notebook groups customers by total transaction-row count and calculates the percentage labelled inactive in each group. The displayed pattern is lower inactivity among customers with higher recorded transaction counts.

This is descriptive. The grouping variable and the inactivity label are both derived from the same historical observation window, so the relationship should not be interpreted as a causal effect.

## 7. Logistic-regression baseline

The notebook trains a logistic-regression model using:

- Total transaction count
- Total list-price value
- Average list-price value per transaction row
- Online-order share
- Calculated margin

It uses a stratified random 80/20 train-test split and reports ROC-AUC and a classification report. The executed notebook records a test ROC-AUC of 0.724 and churn recall of 0.199.

### Model limitation

These features are calculated over the same full period used to assign the inactivity label. This introduces temporal / target leakage and makes the model unsuitable as a genuine forward-looking churn predictor. Its score is retained as an exploratory demonstration only.

A sound predictive design would set a historical snapshot date, calculate features only from data on or before that date, then label customers using activity in a separate future outcome window. Evaluation should preserve chronological order.
