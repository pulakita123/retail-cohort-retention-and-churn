# Data Dictionary

## Source file

The notebook reads the first worksheet from `Online Retail.xlsx`. The workbook contains bicycle-retail transaction and product attributes. The repository filename does not establish the original publisher, licence or provenance; those details should be confirmed separately before redistributing the data.

The analysis filters the table to `order_status = Approved`, leaving **19,821 approved transaction rows** and **3,493 distinct customers** in the recorded output.

## Source fields used

| Field | Meaning in this analysis | Use |
|---|---|---|
| `transaction_id` | Identifier for a transaction row | Count transactions per customer |
| `product_id` | Product identifier | Available for product-level extensions |
| `customer_id` | Customer identifier | Customer grouping and cohort assignment |
| `transaction_date` | Date of the transaction | Cohort month, last purchase date and recency |
| `online_order` | Online-order indicator (0/1 in the sample) | Customer online-order share |
| `order_status` | Transaction status | Filter to Approved |
| `brand` | Product brand | Available for brand-level analysis |
| `product_line` | Product line | Available for product mix analysis |
| `product_class` | Product class | Product segmentation |
| `product_size` | Product size | Product segmentation |
| `list_price` | Listed price recorded for the transaction | Customer-level spend proxy and average order value proxy |
| `standard_cost` | Standard product cost | Margin proxy |
| `product_first_sold_date` | Product first-sold date value | Not used in the core cohort calculation |

## Derived fields

| Derived field | Definition |
|---|---|
| `OrderMonth` | Transaction date converted to calendar month |
| `CohortMonth` | Earliest approved transaction month for each customer |
| `CohortIndex` | Number of calendar months between transaction month and cohort month; 0 is the first-purchase month |
| `last_order_date` | Latest approved transaction date for the customer |
| `total_orders` | Count of approved transaction rows per customer |
| `total_spend` | Sum of `list_price` across the customer's approved transaction rows |
| `online_share` | Mean of the online-order indicator for the customer's rows |
| `recency_days` | Days between the dataset's maximum transaction date and the customer's last transaction date |
| `Churned` | 1 when `recency_days > 90`; otherwise 0 |
| `avg_order_value` | `total_spend / total_orders`; transaction-row average, not necessarily basket-level AOV |
| `margin` | Sum of `list_price - standard_cost` across the customer's approved transaction rows |

## Important grain and terminology notes

- The source table appears to be at transaction-line / transaction-record grain. The notebook counts `transaction_id` rows as orders. Before interpreting this as distinct customer orders, confirm that each transaction ID represents one complete order rather than an item-level line.
- `total_spend` is based on `list_price`, not a verified net sales field after discounts, returns, taxes or shipping.
- `margin` is a simple list-price-minus-standard-cost measure. It should not be described as audited profit.
- The workbook's provenance and reuse licence have not been verified.
