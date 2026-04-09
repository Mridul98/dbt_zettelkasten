# [Model Name] (e.g., int_payments_pivoted)
**UID:** [YYYYMMDDHHMM]
**Type:** Reference / dbt Model
**Last Verified Commit:** [Git Commit Hash or "N/A"]

## 1. Grain & Core Purpose
* **Grain:** One row per `[grain_column]`.
* **Purpose:** [1-2 sentences explaining the core objective of this model in the pipeline.]

## 2. Column Lineage & Logic
* **`[column_1]`**: [Primary key / Business description]
  * **Logic:** [The exact SQL snippet or plain English explanation of the transformation, e.g., `COALESCE(amount, 0) * exchange_rate`]
  * **Origin:** [Upstream Model Name](../staging/YYYYMMDDHHMM-stg_upstream_model.md) -> `[upstream_column_name]`
* **`[column_2]`**: [Description]
  * **Logic:** Passthrough.
  * **Origin:** [Upstream Model Name](../staging/YYYYMMDDHHMM-stg_upstream_model.md) -> `[column_2]`

## 3. Transformation Evolution (Chronological)
1. **CTE `[name]`:** Reads from [Upstream Model Name](../staging/YYYYMMDDHHMM-stg_upstream_model.md).
2. **CTE `[name]`:** 
    * [Explain overall logical architecture, e.g., complex joins, fan-outs, or window functions. Column specific math goes in Section 2.]

## 4. Lineage (The DAG)
* **Depends On (Upstream):** 
  * [Upstream Model Name](../staging/YYYYMMDDHHMM-stg_upstream_model.md)
* **Used By (Downstream):** 
  * [Downstream Model Name](../reporting/YYYYMMDDHHMM-downstream_model_report.md)

## 5. Deprecation Notes (Tombstone Pattern)
*(Leave empty unless model is deprecated)*
* **Deprecated On:** [Date]
* **Reason:** [Why was this removed? What replaced it?]