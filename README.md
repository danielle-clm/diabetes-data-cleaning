# 🏥 Project 2 — Diabetes Hospital Dataset: Data Cleaning

## 📌 Project Overview
This project focuses on cleaning a real-world diabetes hospital readmission datasetG using **Excel and Power Query**. The goal was to transform a raw, messy dataset into a clean, analysis-ready table while documenting every decision made along the way.
---

## 🛠️ Tools Used
- Microsoft Excel
- Power Query (Power Query Editor)
- Find & Replace
- Conditional Formatting

---

## 📊 Dataset
- **Source:** UCI Machine Learning Repository — Diabetes 130-US Hospitals (1999-2008)
- **Original size:** 101,766 rows × 50 columns
- **Final size:** 101,766 rows × 45 columns

---

## ✅ What Was Done

### Power Query
- Loaded the structured dataset into Power Query
- Imported `IDS_mapping` dictionary (manually split into 3 sheets first to avoid stacked table conflicts)
- Performed 3 **left outer joins** to replace numeric IDs with readable labels:
  - `admission_type_id` → `admission_type_label`
  - `discharge_disposition_id` → `discharge_label`
  - `admission_source_id` → `admission_source_label`
- Replaced all `?` placeholder values with `null` to accurately represent missing data
- Removed 8 columns (details below)

### Column Removal Decisions
| Column | % Missing | Reason |
|--------|-----------|--------|
| `weight` | 97% | Statistically unreliable |
| `payer_code` | 100% | Entirely empty |
| `max_glu_serum` | 94.7% | Too incomplete to analyze |
| `A1Cresult` | 83.3% | Too incomplete — medically expected to be sparse |
| `medical_specialty` | 49% | Redundant with label columns |
| `admission_type_id` | — | Replaced by label column |
| `discharge_disposition_id` | — | Replaced by label column |
| `admission_source_id` | — | Replaced by label column |

### Excel Cleaning
- Fixed `AfricanAmerican` → `African American` using Find & Replace
- Removed bracket formatting from age column (`[10-20)` → `10-20`) — formatted column as Text first to prevent Excel date conversion
- Grouped age ranges into descriptive categories: Young & Under, Young Adult, Middle Aged, Senior, Elderly
- Verified zero duplicate `encounter_id` values using Conditional Formatting
- Left ICD-9 diagnosis codes (`diag_1`, `diag_2`, `diag_3`) as-is — V-codes and E-codes are intentionally different formats

---

## ⚠️ Challenges & Lessons Learned

**1. Stacked lookup tables**
The mapping file had 3 tables stacked in one CSV. Filtering by number failed because IDs repeat across sections. Fixed by manually splitting the file into 3 sheets before importing.

**2. Power Query table locks formulas out**
Typing Excel formulas directly inside a Power Query-loaded table triggers errors. Learned to work outside the table or on a separate sheet.

**3. Excel memory issues on large datasets**
Inserting columns in a 100K+ row table caused memory errors. Switched to Find & Replace for bulk operations.

**4. Date conversion trap**
After removing brackets from age, Excel auto-converted `10-20` to `Oct-20`. Fixed by formatting the column as Text *before* running Find & Replace.

**5. Smart quotes in copy-pasted formulas**
Formulas pasted from external sources used smart quotes instead of straight quotes, causing errors. Always retype formulas manually in Excel.

