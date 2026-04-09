# int_sales_funnel
**UID:** 202404101307
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `deal_id`, `funnel_step`, and `first_entry_date`.
* **Purpose:** Unifies deal stage progressions and deal activities into a single comprehensive timeline of funnel steps for each deal.

## 2. Column Lineage & Logic
* **`deal_id`**: The deal identifier.
  * **Logic:** Passthrough.
  * **Origin:** [int_deal_stage_progression](../intermediate/202404101215-int_deal_stage_progression.md) AND [int_deal_activities](../intermediate/202404101306-int_deal_activities.md)
* **`deal_activity_id`**: ID of the activity (NULL if the step comes from a stage change).
  * **Logic:** Passthrough.
  * **Origin:** [int_deal_activities](../intermediate/202404101306-int_deal_activities.md)
* **`funnel_stage_name`**: Human-readable name of the stage or activity.
  * **Logic:** Passthrough.
  * **Origin:** [int_deal_stage_progression](../intermediate/202404101215-int_deal_stage_progression.md) AND [int_deal_activities](../intermediate/202404101306-int_deal_activities.md)
* **`funnel_step`**: The step identifier (e.g., '1', '2.1', '3').
  * **Logic:** Casts `stage_id` to TEXT and merges with `deal_activity_type_stage`.
  * **Origin:** [int_deal_stage_progression](../intermediate/202404101215-int_deal_stage_progression.md) -> `stage_id` AND [int_deal_activities](../intermediate/202404101306-int_deal_activities.md) -> `deal_activity_type_stage`
* **`first_entry_date`**: The timestamp when the deal entered this step.
  * **Logic:** Passthrough.
  * **Origin:** [int_deal_stage_progression](../intermediate/202404101215-int_deal_stage_progression.md) -> `first_entry_date_for_stage` AND [int_deal_activities](../intermediate/202404101306-int_deal_activities.md) -> `deal_activity_due_to`
* **`funnel_month`**: The last day of the month corresponding to `first_entry_date`.
  * **Logic:** `(DATE_TRUNC('MONTH', first_entry_date) + INTERVAL '1 month' - INTERVAL '1 day')::DATE`
  * **Origin:** Internal derivation from `first_entry_date`

## 3. Transformation Evolution (Chronological)
1. **CTE `deal_stages`:** 
    * Reads from [int_deal_stage_progression](../intermediate/202404101215-int_deal_stage_progression.md).
2. **CTE `deal_activities`:** 
    * Reads from [int_deal_activities](../intermediate/202404101306-int_deal_activities.md).
    * Filters for `is_deal_activity_done = TRUE`.
3. **CTE `sales_funnel`:** 
    * `UNION ALL` merges the `deal_stages` and `deal_activities` CTEs together, normalizing the schema into a unified funnel timeline.
4. **Final Select:** 
    * Calculates `funnel_month`.

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * [int_deal_stage_progression](../intermediate/202404101215-int_deal_stage_progression.md)
  * [stg_pipedrive_deal_stages](../staging/202404101230-stg_pipedrive_deal_stages.md)
  * [int_deal_activities](../intermediate/202404101306-int_deal_activities.md)
* **Used By (Downstream):** 
  * [int_sales_funnel_monthly](../intermediate/202404101309-int_sales_funnel_monthly.md)