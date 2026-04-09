# stg_pipedrive_deal_lost_reason
**UID:** 202404101304
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `lost_reason_id`.
* **Purpose:** Dimension table extracting specific lost reason labels from the generic fields table.

## 2. Column Lineage & Logic
* **`lost_reason_id`**: Primary key for the specific reason.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_fields_snapshot` -> `field_key_id`
* **`actual_lost_reason`**: Human-readable text explaining the reason.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_fields_snapshot` -> `field_key_label`

## 3. Transformation Evolution (Chronological)
1. **Final Select:** 
    * Reads from `pipedrive_deal_fields_snapshot`.
    * Hard filters `field_key = 'lost_reason'` to extract only lost reason records.
    * Filters out invalidated records (`dbt_valid_to IS NULL`).

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * `pipedrive_deal_fields_snapshot`