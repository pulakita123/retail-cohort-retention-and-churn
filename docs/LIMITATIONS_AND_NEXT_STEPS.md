# Limitations and Next Steps

## Current limitations

### 1. Historical observation window

The transaction dates cover 1 January to 30 December 2017. A one-year window limits how much long-term retention can be observed, especially for customers acquired later in the year.

### 2. Inactivity is a proxy, not a confirmed churn definition

The notebook labels a customer as churned when more than 90 days have passed since their last approved transaction, measured against the final transaction date. The threshold is an analytical choice. A real business should define churn based on its purchase cycle, customer contract and operating model.

### 3. Right-censoring and unequal follow-up

Customers acquired late in the dataset have less time in which to make a repeat purchase. Likewise, a customer whose last purchase is close to the dataset end has less observable follow-up. Cohort comparisons should account for this unequal observation time.

### 4. Transaction grain needs confirmation

The notebook counts transaction rows as orders. Before using order frequency or average order value in business reporting, confirm that each `transaction_id` represents a complete order and not a product line within a basket.

### 5. Spend and margin are proxies

Spend is summed from `list_price`; margin is calculated as `list_price - standard_cost`. These fields may not account for discounts, returns, taxes, shipping, promotions or other financial adjustments. They should not be described as audited revenue or profit.

### 6. Model leakage

The logistic-regression features are calculated over the full observation period, while the target is derived from the customer's last transaction within that same period. This creates temporal leakage and prevents the model from being interpreted as a genuine forward-looking churn predictor.

### 7. Class imbalance and low churn recall

The reported churn recall is 0.199. Accuracy and ROC-AUC alone can hide poor performance on the churn class. A retention use case should consider recall, precision, PR-AUC, confusion matrices, calibration and the cost of false positives versus false negatives.

### 8. Data provenance and licence

The workbook is named `Online Retail.xlsx`, but the fields visible in the notebook describe bicycle-retail products and transactions. The original source, publisher and licence have not been independently verified. Confirm permission to redistribute the workbook before treating it as a public portfolio asset.

## Recommended next steps

1. **Create a time-based modelling dataset:** choose a snapshot date and calculate features only from transactions before that date.
2. **Define a future outcome window:** for example, determine whether a customer purchases in the next 90 days, with a clearly stated observation horizon.
3. **Use chronological validation:** train on earlier snapshots and test on later snapshots.
4. **Compare churn thresholds:** test 60-, 90- and 120-day inactivity definitions and show how the customer count changes.
5. **Analyse repeat-purchase timing:** measure days from first to second purchase and the share of customers who reach a second purchase.
6. **Add revenue retention:** calculate cohort revenue and margin retention alongside customer-count retention.
7. **Improve data-quality checks:** investigate missing customer IDs, duplicate transaction identifiers, invalid dates, status values and negative or implausible prices.
8. **Improve segmentation:** compare cohort behaviour by brand, product line, online-order behaviour and customer value, where sample sizes are sufficient.
