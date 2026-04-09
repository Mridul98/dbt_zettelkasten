# int_funnel_kpi_mapping
**UID:** 202404101308
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `funnel_step`.
* **Purpose:** Acts as a master lookup table that defines all valid funnel steps across both deal stages and activity types.

## 2. Column Lineage & Logic
* **`funnel_step`**: The step identifier (e.g., '1', '2.1').
  * **Logic:** Passthrough from UNION.
  * **Origin:** [stg_pipedrive_deal_activity_types](../staging/202404101301-stg_pipedrive_deal_activity_types.md) -> `deal_activity_type_stage` AND [stg_pipedrive_deal_stages](../staging/202404101230-stg_pipedrive_deal_stages.md) -> `deal_stage_id`
* **`funnel_stage_name`**: Human-readable name of the step.
  * **Logic:** Passthrough from UNION.
  * **Origin:** [stg_pipedrive_deal_activity_types](../staging/202404101301-stg_pipedrive_deal_activity_types.md) -> `deal_activity_type_name` AND [stg_pipedrive_deal_stages](../staging/202404101230-stg_pipedrive_deal_stages.md) -> `deal_stage_name`

## 3. Transformation Evolution (Chronological)
1. **Final Select:** 
    * Uses a `UNION` to combine distinct step definitions.
    * Filters out NULL stages.

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * [stg_pipedrive_deal_activity_types](../staging/202404101301-stg_pipedrive_deal_activity_types.md)
  * [stg_pipedrive_deal_stages](../staging/202404101230-stg_pipedrive_deal_stages.md)
* **Used By (Downstream):** 
  * [rep_sales_funnel_monthly](../reporting/202404101310-rep_sales_funnel_monthly.md) (assumed/will verify)