---
name: dbt-zettelkasten
description: Document dbt models, trace data lineage, or query the Data Engineering knowledge base.
---
# Skill: dbt Zettelkasten

## Core Directive
Before starting any documentation task, you MUST read and adhere to the project's official rulebook bundled with this skill. 

*Note to Agent: The guide (`DBT-ZETTELKASTEN-GUIDE.md`) and the `templates/` directory are located in the exact same folder as this SKILL.md file. Use your knowledge of the skill's file path to locate and read them.*

## Quick Reference (Sourced from the Guide)
- **Templates:** Always use `templates/template-dbt-model.md` for new repo notes.
- **UIDs:** Strict naming format: `YYYYMMDDHHMM-[prefix]_descriptive_title_[suffix].md`.
- **Column Lineage:** EVERY column must have `Logic:` and `Origin:` details linking to upstream models.
- **Tombstone Pattern:** NEVER delete deprecated notes. Rename to `...-DEPRECATED-...` and fill out the deprecation section.

## CLI Query Engine (Sourced from the Guide)
Utilize these specific `rg` commands to navigate the Zettelkasten:
- **Trace Column Origin:** `rg -A 2 "\*\*\`[column_name]\`\*\*" 03-repos/`
- **Find Dependencies (1st Degree):** `rg "[UID]"`
- **Scope to Layer:** `rg "search_term" 03-repos/sales-analytics/reporting/`
## Mandatory Triad Documentation
For every new dbt model documented, the agent MUST generate three interconnected notes:
1. A Model Note in `03-repos/` (using `template-dbt-model.md`).
2. A Fleeting Note in `01-fleeting/` (using `template-fleeting.md`) capturing immediate observations.
3. A Knowledge Note in `02-knowledge/` (using `template-knowledge.md`) distilling the core business/architectural concept.
