# int_sales_funnel_monthly
**UID:** 202404101309
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `funnel_month` and `funnel_step`.
* **Purpose:** Aggregates the unified sales funnel timeline into monthly snapshots, counting how many distinct deals hit each step in a given month.

## 2. Column Lineage & Logic
* **`funnel_month`**: The month identifier (end of month date).
  * **Logic:** Passthrough.
  * **Origin:** [int_sales_funnel](../intermediate/202404101307-int_sales_funnel.md) -> `funnel_month`
* **`funnel_stage_name`**: Name of the funnel step.
  * **Logic:** Passthrough.
  * **Origin:** [int_sales_funnel](../intermediate/202404101307-int_sales_funnel.md) -> `funnel_stage_name`
* **`funnel_step`**: The step identifier.
  * **Logic:** Passthrough.
  * **Origin:** [int_sales_funnel](../intermediate/202404101307-int_sales_funnel.md) -> `funnel_step`
* **`deals_in_stage`**: The number of unique deals that reached this step during the month.
  * **Logic:** `COUNT(DISTINCT deal_id)` grouped by `funnel_month`, `funnel_stage_name`, and `funnel_step`.
  * **Origin:** [int_sales_funnel](../intermediate/202404101307-int_sales_funnel.md) -> `deal_id`

## 3. Transformation Evolution (Chronological)
1. **CTE `sales_funnel`:** 
    * Reads from [int_sales_funnel](../intermediate/202404101307-int_sales_funnel.md).
2. **Final Select:** 
    * Aggregates the unified timeline to the monthly grain.

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * [int_sales_funnel](../intermediate/202404101307-int_sales_funnel.md)
* **Used By (Downstream):** 
  * [rep_sales_funnel_monthly](../reporting/202404101310-rep_sales_funnel_monthly.md)