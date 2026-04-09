# int_deal_stage_progression
**UID:** 202404101215
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `deal_id` per `stage_id`.
* **Purpose:** Figures out when a deal entered each stage for the *first time*. 

## 2. Column Lineage & Logic
* **`deal_id`**: The unique identifier for the deal.
  * **Logic:** Passthrough.
  * **Origin:** [stg_pipedrive_deal_changes](../staging/202404101200-stg_pipedrive_deal_changes.md) -> `deal_id`
* **`stage_id`**: The ID of the pipeline stage.
  * **Logic:** Passthrough.
  * **Origin:** [stg_pipedrive_deal_changes](../staging/202404101200-stg_pipedrive_deal_changes.md) -> `stage_id`
* **`first_entry_date_for_stage`**: The earliest timestamp a deal was recorded in this specific stage.
  * **Logic:** `MIN(change_time)` grouped by `deal_id` and `stage_id`.
  * **Origin:** [stg_pipedrive_deal_changes](../staging/202404101200-stg_pipedrive_deal_changes.md) -> `change_time`
* **`deal_stage_name`**: The human-readable name of the stage.
  * **Logic:** Looked up via join.
  * **Origin:** [stg_pipedrive_deal_stages](../staging/202404101230-stg_pipedrive_deal_stages.md) -> `deal_stage_name`

## 3. Transformation Evolution (Chronological)
1. **CTE `first_deal_stage_entry`:** 
    * Reads from [stg_pipedrive_deal_changes](../staging/202404101200-stg_pipedrive_deal_changes.md).
    * Handles the initial grouping and aggregation to find the first entry date.
2. **Final Select:** 
    * Joins to [stg_pipedrive_deal_stages](../staging/202404101230-stg_pipedrive_deal_stages.md) to attach the human-readable `deal_stage_name`.

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * [stg_pipedrive_deal_changes](../staging/202404101200-stg_pipedrive_deal_changes.md)
  * [stg_pipedrive_deal_stages](../staging/202404101230-stg_pipedrive_deal_stages.md)
* **Used By (Downstream):** 
  * N/A (For this walkthrough)