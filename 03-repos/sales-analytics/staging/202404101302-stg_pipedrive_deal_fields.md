# stg_pipedrive_deal_fields
**UID:** 202404101302
**Type:** Reference / dbt Model
**Last Verified Commit:** N/A

## 1. Grain & Core Purpose
* **Grain:** One row per `deal_field_id`.
* **Purpose:** Provides definitions and labels for custom fields within the Pipedrive CRM.

## 2. Column Lineage & Logic
* **`deal_field_id`**: Primary key for the field definition.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_fields_snapshot` -> `field_id`
* **`deal_field_key`**: System key representing the field.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_fields_snapshot` -> `field_key`
* **`deal_field_name`**: Human-readable name of the field.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_fields_snapshot` -> `field_name`
* **`deal_field_value_options`**: JSON/String containing possible values or labels for this field.
  * **Logic:** Passthrough alias.
  * **Origin:** `pipedrive_deal_fields_snapshot` -> `field_key_label`

## 3. Transformation Evolution (Chronological)
1. **Final Select:** 
    * Reads from `pipedrive_deal_fields_snapshot`.
    * Filters out invalidated records (`dbt_valid_to IS NULL`).

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * `pipedrive_deal_fields_snapshot`