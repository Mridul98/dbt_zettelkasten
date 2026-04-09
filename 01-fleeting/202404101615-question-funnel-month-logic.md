# Question: Funnel Month Logic
**UID:** 202404101615
**Type:** Fleeting / Scratchpad

## Context
Reviewing the month truncation logic in [int_sales_funnel](../03-repos/sales-analytics/intermediate/202404101307-int_sales_funnel.md).

## Observation / Question
The `funnel_month` calculation uses: `(DATE_TRUNC('MONTH', first_entry_date) + INTERVAL '1 month' - INTERVAL '1 day')::DATE`. 
This explicitly calculates the *last day of the month*. Does this logic hold up correctly during leap years (e.g., February 29th)? 

## Next Steps
- [ ] Run a quick manual query in Snowflake/Postgres to verify the output for `2024-02-15` (leap year).