# Laptop Dataset Cleaning with SQL

A MySQL data cleaning project that turns a raw, text-heavy laptop dataset into a structured, analysis-ready table. Every step is done in SQL.

## Overview

| | Raw dataset | Cleaned dataset |
|---|---|---|
| Rows | 1,272 | 1,241 |
| Columns | 12 | 23 |
| Format | Mostly text (`8GB`, `1.37kg`, `Intel Core i5 2.3GHz`) | Numeric types and separate feature columns |

The raw data had 29 exact duplicate rows, which were removed.

##  Repository Structure

| File | Description |
|------|-------------|
| `laptop_dataset.csv` | Original raw dataset |
| `Cleaned_laptop_dataset.csv` | Final cleaned dataset |
| `data_cleaning.sql` | All SQL queries used for cleaning |

##  Cleaning Steps

1. **Backup**: created `laptop_backup` before changing anything.
2. **Initial checks**: row count, table size, and null check across all columns.
3. **Renaming**: `Unnamed: 0` → `id`.
4. **Duplicates**: found and deleted duplicate rows using `COUNT() OVER (PARTITION BY ...)` and `ROW_NUMBER()`.
5. **Ram**: removed the `GB` suffix and converted to `INTEGER`.
6. **Weight**: removed the `kg` suffix, replaced the `?` placeholder with 0, and converted to `DECIMAL(5,2)`.
7. **Price**: rounded and converted to `INTEGER`.
8. **OpSys**: standardized labels into `windows`, `mac`, `linux`, `chrome`, `android`, and `na`.
9. **ScreenResolution**: split into `ScreenWidth`, `ScreenHeight`, `ips_panel`, `retina_display`, `touchscreen`, and `resolution_category`.
10. **Cpu**: split into `cpu_brand`, `cpu_family`, `cpu_model`, and `cpu_speed_ghz`.
11. **Memory**: split into `memory_type` (ssd / hdd / hybrid / flash_storage), `primary_storage`, and `secondary_storage`.
12. **Gpu**: split into `gpu_brand` and `gpu_name`.

## 🗂️ Cleaned Dataset Columns

`id`, `Company`, `TypeName`, `Inches`, `cpu_brand`, `cpu_family`, `cpu_speed_ghz`, `cpu_model`, `Ram`, `memory_type`, `primary_storage`, `secondary_storage`, `gpu_brand`, `gpu_name`, `OpSys`, `Weight`, `Price`, `ScreenWidth`, `ScreenHeight`, `ips_panel`, `retina_display`, `resolution_category`, `touchscreen`

## 🛠️ SQL Concepts Used

- `CASE` expressions for categorization
- String functions: `SUBSTRING_INDEX`, `REPLACE`, `LTRIM`, `TRIM`, `CONCAT_WS`
- Regex: `REGEXP_SUBSTR`
- Window functions: `ROW_NUMBER()`, `COUNT() OVER`, `MIN() OVER`
- CTEs with `UPDATE ... JOIN`
- DDL: `ALTER TABLE` (add, rename, modify, drop columns)

##  How to Use

1. Import `laptop_dataset.csv` into MySQL as a table named `laptop`.
2. Run `data_cleaning.sql` section by section.
3. Compare your result with `Cleaned_laptop_dataset.csv`.

##  Known Limitations

- The script contains some exploratory and corrective statements (for example, columns added and later dropped), so run it in order rather than picking single queries.
- Weight values of `?` were set to `0` as a placeholder. Treat these as missing in analysis.
- `resolution_category` is `other` for resolutions without a named label (such as Retina or plain `1440x900`). Use `ScreenWidth` and `ScreenHeight` for exact values.
- `gpu_name` has a leading space from the `REPLACE` step, so apply `TRIM()` if needed.
- The cleaned CSV has 1,241 rows. The script's duplicate removal alone gives 1,243, so 2 further rows were removed or changed outside the script.

##  Outcome

A consistent dataset with proper data types and meaningful features, ready for EDA, visualization, or machine learning (for example, price prediction).


---

⭐ If you found this useful, feel free to star the repo. Feedback is welcome!
