# Business Problem

## Context

A retailer needs more than a count of completed transactions to understand customer health. The business also needs to know whether newly acquired customers return, how quickly repeat purchasing declines, and whether customer activity differs across acquisition groups.

This project examines those questions using a historical transaction-level retail dataset.

## Problem statement

The analysis aims to turn transaction records into a customer-lifecycle view that can support retention discussions. It focuses on two connected questions:

- **Cohort retention:** Do customers acquired in different months show different repeat-purchase patterns?
- **Inactivity risk:** Which customer behaviours are associated with having no approved purchase for an extended period?

## Business questions

1. How many unique customers made at least one approved transaction?
2. How many customers from each first-purchase month were active in subsequent months?
3. At what point does observed cohort activity decline?
4. What proportion of customers had no approved purchase for more than 90 days before the final observed transaction date?
5. How does the inactivity rate vary by transaction count?
6. How well does a simple logistic-regression baseline distinguish customers labelled inactive under this rule?

## Intended users

- CRM and customer-retention teams
- E-commerce and retail managers
- Marketing analysts
- Business analysts supporting customer lifecycle decisions

## Why it matters

If customers do not return after their first purchase, acquisition spending may not translate into durable customer value. Cohort analysis helps separate the behaviour of newer customers from that of older cohorts. An inactivity measure can also help teams investigate segments that may need re-engagement.

## Scope and interpretation

The current project is exploratory. It uses approved transactions from the workbook and defines churn as more than 90 days without an approved transaction by the end of the dataset. This is a practical inactivity proxy, not a confirmed business definition of churn.

The analysis identifies associations, not causal effects. For example, customers with more transactions have lower observed inactivity rates, but this does not prove that increasing order count by itself prevents churn.
