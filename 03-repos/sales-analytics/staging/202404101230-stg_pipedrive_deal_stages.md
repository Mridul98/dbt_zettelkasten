# stg_pipedrive_deal_stages
**UID:** 202404101230
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `deal_stage_id`.
* **Purpose:** Provides a clean list of valid, active pipeline stages and their names.

## 2. Column Lineage & Logic
* **`deal_stage_id`**: Primary key.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_stages_snapshot` -> `stage_id`
* **`deal_stage_name`**: The human-readable name of the stage.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_stages_snapshot` -> `stage_name`

## 3. Transformation Evolution (Chronological)
1. **Final Select:** 
    * Reads from `pipedrive_deal_stages_snapshot`.
    * Filters out invalidated or null stages (`dbt_valid_to IS NULL`).

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * `pipedrive_deal_stages_snapshot` (dbt Snapshot)
* **Used By (Downstream):** 
  * [int_deal_stage_progression](../intermediate/202404101215-int_deal_stage_progression.md)