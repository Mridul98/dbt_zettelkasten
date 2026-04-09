# rep_sales_funnel_monthly
**UID:** 202404101310
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `month` and `funnel_step`.
* **Purpose:** Final presentation layer for the monthly sales funnel, formatted for BI tools to track the volume of deals at each KPI stage per month.

## 2. Column Lineage & Logic
* **`month`**: The month of the funnel activity.
  * **Logic:** Passthrough alias.
  * **Origin:** [int_sales_funnel_monthly](../intermediate/202404101309-int_sales_funnel_monthly.md) -> `funnel_month`
* **`kpi_name`**: The human-readable name of the KPI / funnel stage.
  * **Logic:** Passthrough alias.
  * **Origin:** [int_funnel_kpi_mapping](../intermediate/202404101308-int_funnel_kpi_mapping.md) -> `funnel_stage_name`
* **`funnel_step`**: The step identifier.
  * **Logic:** Passthrough.
  * **Origin:** [int_sales_funnel_monthly](../intermediate/202404101309-int_sales_funnel_monthly.md) -> `funnel_step`
* **`deals_count`**: The number of deals that reached this step in this month.
  * **Logic:** Passthrough alias.
  * **Origin:** [int_sales_funnel_monthly](../intermediate/202404101309-int_sales_funnel_monthly.md) -> `deals_in_stage`

## 3. Transformation Evolution (Chronological)
1. **Final Select:** 
    * Reads from [int_sales_funnel_monthly](../intermediate/202404101309-int_sales_funnel_monthly.md).
    * Joins to [int_funnel_kpi_mapping](../intermediate/202404101308-int_funnel_kpi_mapping.md) on `funnel_step`.
    * Handles final presentation aliasing.

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * [int_sales_funnel_monthly](../intermediate/202404101309-int_sales_funnel_monthly.md)
  * [int_funnel_kpi_mapping](../intermediate/202404101308-int_funnel_kpi_mapping.md)
* **Used By (Downstream):** 
  * N/A (End of Pipeline)