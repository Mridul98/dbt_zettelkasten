# The Unified Sales Funnel Architecture
**UID:** 202404101500
**Type:** Permanent / Domain Knowledge

## Concept
The "Sales Funnel" in this repository is not merely a sequence of pipeline stage changes. It is a hybrid timeline. To accurately measure conversion, the architecture unifies two disparate entities:
1. **Passive Stage Progressions:** A deal naturally moving from 'Lead' to 'Qualified'.
2. **Active Deal Activities:** Specific actions taken by sales reps (like 'Sales Call 1') that the business considers equivalent to reaching a funnel milestone.

By normalizing both entities into a single timeline, we can track the true velocity of a deal, even if the CRM's native stage ID wasn't explicitly updated.

## Practical Implementation
This concept is implemented via a `UNION ALL` in the intermediate funnel logic. It takes the earliest entry date from stage progressions and unions it with completed deal activities. 

* **Implementation Model:** [int_sales_funnel](../03-repos/sales-analytics/intermediate/202404101307-int_sales_funnel.md)

## Related Notes
* [Funnel KPI Mapping Rules](./202404101530-funnel-kpi-mapping-rules.md)